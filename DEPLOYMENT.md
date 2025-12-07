# Deployment Guide - AI Chatbot

This application can be deployed to IIS (Windows Server) or Azure App Service.

## Prerequisites

- Node.js 18 or higher
- npm or yarn
- For IIS: URL Rewrite module installed
- For Azure: Azure App Service with Node.js runtime

## Build for Production

```bash
npm run build
```

This creates a production build in the `dist` folder.

## Environment Variables

Set these environment variables in your hosting environment:

| Variable | Description | Required |
|----------|-------------|----------|
| `AI_INTEGRATIONS_OPENAI_BASE_URL` | OpenAI API base URL | Yes |
| `AI_INTEGRATIONS_OPENAI_API_KEY` | OpenAI API key | Yes |
| `NODE_ENV` | Set to `production` | Recommended |
| `PORT` | Server port (default: 5000) | Optional |

## IIS Deployment

1. **Install Prerequisites**
   - Install Node.js on the server
   - Install [iisnode](https://github.com/azure/iisnode)
   - Install URL Rewrite module for IIS

2. **Deploy Files**
   - Copy the `dist` folder contents to your IIS site directory
   - Copy `web.config` to the site root
   - Copy `iisnode.yml` to the site root

3. **Configure IIS**
   - Create a new website or application in IIS Manager
   - Point to the deployment directory
   - Ensure the Application Pool uses "No Managed Code"

4. **Set Environment Variables**
   - Open IIS Manager
   - Select your site
   - Open "Configuration Editor"
   - Navigate to `system.webServer/aspNetCore`
   - Add environment variables

## Azure App Service Deployment

### Option 1: Deploy via Azure CLI

```bash
# Login to Azure
az login

# Create resource group (if needed)
az group create --name myResourceGroup --location eastus

# Create App Service plan
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux

# Create web app
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name my-ai-chatbot --runtime "NODE|18-lts"

# Configure startup command
az webapp config set --resource-group myResourceGroup --name my-ai-chatbot --startup-file "node dist/index.js"

# Set environment variables
az webapp config appsettings set --resource-group myResourceGroup --name my-ai-chatbot --settings AI_INTEGRATIONS_OPENAI_BASE_URL="your-url" AI_INTEGRATIONS_OPENAI_API_KEY="your-key"

# Deploy
az webapp deployment source config-zip --resource-group myResourceGroup --name my-ai-chatbot --src dist.zip
```

### Option 2: Deploy via GitHub Actions

Create `.github/workflows/azure-deploy.yml`:

```yaml
name: Deploy to Azure

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Install and Build
        run: |
          npm ci
          npm run build
          
      - name: Deploy to Azure
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'my-ai-chatbot'
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
          package: ./dist
```

### Option 3: Deploy via Azure Portal

1. Go to Azure Portal
2. Create a new Web App resource
3. Select Node.js runtime
4. Configure deployment source (GitHub, Local Git, etc.)
5. Set environment variables in Configuration > Application settings

## Health Check

The application exposes a health endpoint:

```
GET /api/health
```

Response:
```json
{
  "status": "ok",
  "timestamp": "2024-01-01T00:00:00.000Z"
}
```

Use this endpoint for load balancer health checks and monitoring.

## Troubleshooting

### Common Issues

1. **502 Bad Gateway**
   - Check if Node.js is installed correctly
   - Verify iisnode is installed
   - Check application logs in `iisnode` folder

2. **Chat not responding**
   - Verify AI_INTEGRATIONS_OPENAI_BASE_URL and AI_INTEGRATIONS_OPENAI_API_KEY are set
   - Check network connectivity to OpenAI API

3. **Static files not loading**
   - Ensure URL Rewrite module is installed
   - Verify web.config is in the site root

### Logs

- IIS: Check `iisnode` folder in site directory
- Azure: Use Log Stream in Azure Portal or Application Insights

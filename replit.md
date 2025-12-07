# AI Chatbot Web Application

An intelligent conversational AI chatbot built with React and Express, optimized for deployment on IIS and Azure App Service.

## Overview

This application provides a modern chat interface powered by OpenAI's language models via Replit AI Integrations. Users can have natural conversations with an AI assistant that provides helpful, accurate responses.

## Recent Changes

- **December 2024**: Initial implementation with full chat functionality
- Added dark/light theme support
- Created IIS/Azure deployment configurations
- Implemented responsive design for all device sizes

## User Preferences

- Uses Inter font for clean, modern typography
- Light/dark mode with system preference detection
- Smooth animations and transitions for message bubbles
- Enter to send, Shift+Enter for new line

## Project Architecture

### Frontend (`client/`)
- **React** with TypeScript
- **Tailwind CSS** with Shadcn UI components
- **TanStack Query** for data fetching
- **Wouter** for routing

### Backend (`server/`)
- **Express.js** with TypeScript
- **OpenAI SDK** via Replit AI Integrations
- RESTful API design

### Shared (`shared/`)
- **Zod schemas** for type validation
- Shared TypeScript types

## Key Files

| File | Purpose |
|------|---------|
| `client/src/pages/chat.tsx` | Main chat interface |
| `client/src/components/` | UI components (MessageBubble, ChatInput, etc.) |
| `server/routes.ts` | API endpoints |
| `server/openai.ts` | OpenAI client configuration |
| `web.config` | IIS deployment configuration |
| `DEPLOYMENT.md` | Deployment guide for IIS/Azure |

## API Endpoints

- `POST /api/chat` - Send messages and receive AI responses
- `GET /api/health` - Health check endpoint

## Environment Variables

- `AI_INTEGRATIONS_OPENAI_BASE_URL` - OpenAI API base URL (auto-configured by Replit)
- `AI_INTEGRATIONS_OPENAI_API_KEY` - OpenAI API key (auto-configured by Replit)

## Running Locally

The application runs on port 5000 with both frontend and backend served together.

```bash
npm run dev
```

## Building for Production

```bash
npm run build
```

Output is in the `dist/` folder, ready for IIS or Azure deployment.

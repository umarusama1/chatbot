# AI Chatbot Web Application - Design Guidelines

## Design Approach
**Selected Approach:** Design System-based with Material Design principles
**References:** ChatGPT, Claude, Linear messaging patterns
**Rationale:** Chat interfaces require clarity, accessibility, and instant familiarity. Users expect standard patterns for optimal usability in conversation flows.

## Core Design Elements

### Typography
- **Primary Font:** Inter or Roboto via Google Fonts CDN
- **Message Text:** Regular weight, 16px (text-base)
- **Timestamps:** Light weight, 12px (text-xs), reduced opacity
- **User Names:** Medium weight, 14px (text-sm)
- **Input Field:** Regular weight, 16px (text-base)
- **Headings (if any):** Semibold weight, 20px (text-xl)

### Layout System
**Spacing Primitives:** Tailwind units of 2, 3, 4, 6, 8, 12
- Message padding: p-4
- Message gaps: space-y-3
- Section margins: mb-6, mt-8
- Input container padding: p-3
- Sidebar/header padding: p-4

**Container Strategy:**
- Chat container: max-w-4xl mx-auto, full viewport height
- Message bubbles: max-w-2xl with appropriate left/right positioning
- Input area: Fixed at bottom, full width with max-w-4xl inner container

### Component Library

**1. Chat Layout Structure**
- Full-height viewport layout (h-screen flex flex-col)
- Header bar: Fixed height (h-16), contains title and controls
- Messages area: flex-1 overflow-y-auto with scroll-to-bottom behavior
- Input area: Fixed at bottom, elevated appearance

**2. Message Bubbles**
- User messages: Right-aligned, rounded-2xl, rounded-tr-sm
- AI messages: Left-aligned, rounded-2xl, rounded-tl-sm
- Padding: px-4 py-3
- Max-width constraint to prevent over-stretching
- Avatar icons: 8x8 circular, adjacent to messages

**3. Input Component**
- Multi-line textarea with auto-expand (max 4-5 lines)
- Send button: Circular icon button, positioned bottom-right of input
- Typing indicator: Animated dots below input when AI is responding
- Character: Rounded corners (rounded-xl), border treatment

**4. Header Elements**
- Application title/logo: Left-aligned
- New chat button: Icon button, top-right
- Settings/menu icon: Icon button, adjacent to new chat
- Minimal height with clear separation from chat area

**5. Loading States**
- Typing indicator: Three animated dots in AI message bubble
- Pulse animation for "thinking" state
- Smooth fade-in for new messages (transition-opacity duration-200)

**6. Empty State**
- Centered welcome message with suggested prompts
- Prompt cards: Rounded borders, clickable, hover lift effect
- Grid layout: grid-cols-1 md:grid-cols-2 gap-3

### Interaction Patterns

**Message Flow:**
- Messages appear with subtle fade-in
- Auto-scroll to latest message on new send
- Maintain scroll position when viewing history

**Input Behavior:**
- Auto-focus on mount
- Enter to send, Shift+Enter for new line
- Disable send while processing
- Clear input after successful send

**Responsive Breakpoints:**
- Mobile (base): Single column, full-width messages
- Tablet (md:): Maintain layout, slightly wider bubbles
- Desktop (lg:): Maximum container width, optimal reading line length

### Accessibility
- ARIA labels on all interactive elements
- Keyboard navigation for all actions (Tab, Enter, Escape)
- Focus indicators with 2px outline offset
- Screen reader announcements for new messages
- Sufficient contrast ratios (WCAG AA minimum)
- Focus trap in input area during active typing

### Icons
**Library:** Heroicons via CDN
- Send icon: Paper airplane or arrow
- New chat: Plus or document icon
- Menu: Three horizontal lines
- User avatar: User circle or person icon
- AI avatar: Sparkle or robot icon

### Performance Considerations
- Virtual scrolling for long conversation histories
- Message batching to prevent UI jank
- Debounced typing indicators
- Lazy load older messages on scroll-up

### Special Features
- Code block syntax highlighting (if applicable): Use highlight.js
- Markdown rendering: Use marked.js for formatted responses
- Copy message button: Appears on hover, clipboard icon
- Timestamp display: Relative time ("Just now", "2 min ago")

This design prioritizes clarity, speed, and familiar interaction patterns that users expect from modern chat interfaces, ensuring immediate usability without learning curve.
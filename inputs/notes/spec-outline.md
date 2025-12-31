# Product Specification: Multi-User Shared AI Chat

## 1. Overview

### 1.1 Product Vision

A collaborative AI chat application that enables multiple users (2-4) to interact with a single AI assistant in real-time, sharing context, questions, and responses in a unified conversation experience.

### 1.2 Problem Statement

Current AI chat tools are designed for single-user interactions. Users who want to collaborate with others on AI-assisted tasks must either:
- Share static snapshots of conversations (losing interactivity)
- Manually relay information between separate sessions
- Duplicate queries across multiple accounts

### 1.3 Target Users

- **Couples/Partners**: Joint decision-making, shared research, household management
- **Families**: Parenting advice, family planning, shared knowledge base
- **Small Teams**: Collaborative research, brainstorming, project planning
- **Friends**: Group planning, shared interests, collective problem-solving

## 2. Core Concept

### 2.1 The Idea

A chat interface where:
- Multiple authenticated users join a shared conversation
- One AI agent serves all participants
- All users see the full conversation history
- Messages are attributed to their senders
- The AI understands and addresses different users appropriately

### 2.2 What the User Is Looking For

1. **Commercial SaaS Solution**: A ready-to-use service offering this functionality
2. **Open Source Project**: An existing open-source implementation
3. **Component Libraries**: Chat UI components that support multi-user scenarios
4. **Technical Validation**: Confirmation that this is architecturally feasible

## 3. Feature Set

### 3.1 Core Features (MVP)

| Feature | Description | Priority |
|---------|-------------|----------|
| **Multi-user sessions** | 2-4 users can join a single chat session | P0 |
| **User identification** | AI receives and displays user attribution on all messages | P0 |
| **Real-time sync** | All participants see messages instantly | P0 |
| **Shared context** | Single conversation history accessible to all | P0 |
| **User authentication** | Secure login for each participant | P0 |
| **Session management** | Create, join, leave shared conversations | P0 |

### 3.2 Enhanced Features (V1)

| Feature | Description | Priority |
|---------|-------------|----------|
| **Custom system prompts** | Define AI personality/role (like custom GPTs) | P1 |
| **Invite links** | Generate secure links to invite users | P1 |
| **Notification system** | Alert users when others add to conversation | P1 |
| **Message threading** | Reply to specific messages | P1 |
| **Read receipts** | See when others have read messages | P1 |
| **User presence** | Show who is currently active in chat | P1 |

### 3.3 Advanced Features (Future)

| Feature | Description | Priority |
|---------|-------------|----------|
| **Passive participation** | Observer mode without sending messages | P2 |
| **Chat export** | Export to Markdown, PDF, Google Drive, Confluence | P2 |
| **Voice input** | Speech-to-text for hands-free interaction | P2 |
| **Mobile apps** | Native iOS/Android applications | P2 |
| **AI model selection** | Choose between different LLM providers | P2 |
| **Conversation branching** | Fork conversations into new sessions | P2 |
| **File sharing** | Upload and share files within chat | P2 |
| **Search history** | Search across conversation history | P2 |

## 4. Technical Requirements

### 4.1 Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      Frontend                           │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │   User A    │  │   User B    │  │   User C    │     │
│  │   Client    │  │   Client    │  │   Client    │     │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘     │
└─────────┼────────────────┼────────────────┼────────────┘
          │                │                │
          └────────────────┼────────────────┘
                           │
                    ┌──────▼──────┐
                    │  WebSocket  │
                    │   Server    │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
    ┌─────▼─────┐   ┌──────▼──────┐   ┌─────▼─────┐
    │  Session  │   │   Message   │   │    Auth   │
    │  Manager  │   │   Handler   │   │  Service  │
    └─────┬─────┘   └──────┬──────┘   └───────────┘
          │                │
          │         ┌──────▼──────┐
          │         │  LLM API    │
          │         │  (OpenAI/   │
          │         │  Anthropic) │
          │         └─────────────┘
          │
    ┌─────▼─────┐
    │ Database  │
    │ (Sessions,│
    │  Messages)│
    └───────────┘
```

### 4.2 Key Technical Components

| Component | Technology Options |
|-----------|-------------------|
| **Real-time communication** | WebSockets, Socket.io, Pusher |
| **Frontend framework** | React, Vue, Svelte |
| **Backend** | Node.js, Python (FastAPI), Go |
| **Database** | PostgreSQL, MongoDB |
| **Authentication** | Auth0, Clerk, NextAuth |
| **LLM Integration** | OpenAI API, Anthropic API, OpenRouter |
| **Chat UI** | Custom, Chatbot-UI, LibreChat components |

### 4.3 API Message Format

```json
{
  "session_id": "abc123",
  "message": {
    "id": "msg_789",
    "content": "What smart speakers do you recommend?",
    "sender": {
      "id": "user_456",
      "name": "Daniel",
      "type": "human"
    },
    "timestamp": "2025-12-31T15:00:00Z"
  }
}
```

### 4.4 LLM System Prompt Template

```
You are a helpful AI assistant serving a shared conversation with multiple users.

Participants in this conversation:
- Daniel (user_456)
- Hannah (user_789)

Each message you receive will be prefixed with the sender's name. When responding:
1. Address the specific user who asked the question when appropriate
2. Be aware that other participants can see your response
3. Maintain context from all participants' contributions
4. If relevant, reference what other users have said

Respond naturally and helpfully while acknowledging the collaborative nature of this chat.
```

## 5. Scaling Considerations

### 5.1 Recommended Limits

| Limit | Value | Rationale |
|-------|-------|-----------|
| Max users per session | 4-6 | Context window constraints, UX clarity |
| Max concurrent sessions | Based on infrastructure | Cost management |
| Message rate limit | 20/min per user | API cost control |
| Session timeout | 30 days inactive | Resource cleanup |

### 5.2 Why Not Larger Groups?

- **Context window consumption**: User identification metadata scales linearly
- **Conversation chaos**: Many simultaneous participants create confusion
- **AI response quality**: Harder to address individual needs with many users
- **UX complexity**: More users = harder to follow conversation

## 6. Success Metrics

| Metric | Description |
|--------|-------------|
| **Session creation rate** | How often users create shared sessions |
| **User retention** | Do users return to continue conversations |
| **Messages per session** | Engagement depth |
| **Multi-user participation rate** | % of sessions with 2+ active users |
| **User satisfaction** | NPS, feedback surveys |

## 7. Open Questions for Research

1. **Existing solutions**: Are there any commercial or open-source tools already doing this?
2. **Component availability**: What chat UI libraries support multi-user attribution?
3. **LLM optimization**: Best practices for multi-user system prompts?
4. **Privacy/legal**: Data ownership in shared conversations?
5. **Monetization**: Per-seat vs. per-session vs. flat-rate pricing?

## 8. Next Steps

1. [ ] Research existing commercial solutions
2. [ ] Survey open-source chat projects (LibreChat, Chatbot-UI, etc.)
3. [ ] Evaluate WebSocket frameworks for real-time sync
4. [ ] Prototype basic 2-user chat with user attribution
5. [ ] Test with target users (couples, families)
6. [ ] Iterate based on feedback

---

*Document created from audio recording analysis, December 31, 2025*

# SAM Chat System - Quick Orientation

> **Read this first.** Takes 2 minutes. Saves 2 sessions.

---

## What Is This?

SAM is an AI chat assistant built into Odoo. User types message → system gathers context → calls Claude → streams response back.

---

## The Join Point

**Everything flows through ONE file:**

```
api_communications/sam_chat.py → SAMChat class → process_chat_message_streaming()
```

This is the orchestrator. Start here.

---

## The 14 Files That Matter

### The Flow (in order)

| Step | File | What It Does |
|------|------|--------------|
| 1 | `ai_sam/static/src/js/chat/sam_chat_client.js` | Frontend sends message via SSE |
| 2 | `ai_sam/static/src/js/context/sam_context_gatherer.js` | Frontend gathers location context |
| 3 | `controllers/sam_ai_chat_controller.py` | HTTP endpoint receives request |
| 4 | `api_communications/sam_chat.py` | **THE ORCHESTRATOR** |
| 5 | `api_communications/session_manager.py` | Caches sessions by location |
| 6 | `api_communications/session_context.py` | Builds system prompt + tools |
| 7 | `api_communications/system_prompt.py` | Defines what SAM knows |
| 8 | `api_communications/location_insights.py` | Detects where user is in Odoo |
| 9 | `api_communications/api_services.py` | Calls Claude/OpenAI API |
| 10 | `api_communications/core_tools.py` | CRUD tool definitions + execution |
| 11 | `api_communications/chat_tools.py` | Memory tool definitions |

### Also Important (but secondary)

| File | What It Does |
|------|--------------|
| `models/ai_brain.py` | Legacy tool routing (3000+ lines, being refactored) |
| `models/ai_context_builder.py` | Context building helpers |
| `models/odoo_tools.py` | Odoo data tool definitions |

---

## The Contract (Frontend ↔ Backend)

Frontend calls these endpoints - they don't change:

```
POST /sam_ai/chat/send_streaming  → SSE stream response
POST /sam_ai/chat/send            → JSON response
```

Frontend sends:
```json
{
  "message": "user's question",
  "conversation_id": 123,
  "context_data": {
    "url": "/web#action=123&model=crm.lead&id=45",
    "model": "crm.lead",
    "record_id": 45
  }
}
```

Backend streams back SSE events:
```
event: activity   → {"activity": "thinking"}
event: chunk      → {"text": "Hello, I can"}
event: chunk      → {"text": " help with that."}
event: done       → {"conversation_id": 123}
```

---

## Module Dependency

```
ai_sam_base (backend logic)
     ↑
ai_sam (frontend UI/skin)
```

`ai_sam` depends on `ai_sam_base`. Refactoring `ai_sam_base` internals won't break `ai_sam` as long as HTTP endpoints stay the same.

---

## Where To Look For...

| If you need to... | Look in... |
|-------------------|------------|
| Change how messages are processed | `api_communications/sam_chat.py` |
| Change what SAM knows/personality | `api_communications/system_prompt.py` |
| Add a new tool | `api_communications/core_tools.py` |
| Change location detection | `api_communications/location_insights.py` |
| Debug session issues | `api_communications/session_manager.py` |
| Change frontend chat UI | `ai_sam/static/src/js/sam_chat_vanilla_v2.js` |
| Change how context is gathered (frontend) | `ai_sam/static/src/js/context/sam_context_gatherer.js` |

---

## Key Insight

**Session context is built ONCE per location, then cached.**

Same user + same Odoo location = reuse session (no rebuild).
User navigates to new page = build new session context.

This is why SAM "knows where you are" - the URL/model/record_id from frontend determines the context.

---

## Source Code Locations

```
D:\github_repos\04_samai_user_experience\ai_sam_base\  → Backend
D:\github_repos\04_samai_user_experience\ai_sam\       → Frontend (skin)
```

## Detailed Docs

- [Chat Message Flow - Detailed](./chat_message_flow_DETAIL.md)
- [Chat Message Flow - Diagrams](./chat_message_flow_DIAGRAM.md)

---

*Last updated: 2026-01-30*

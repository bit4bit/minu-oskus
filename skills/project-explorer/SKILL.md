---
name: project-explorer
description: >
  Deep-dive analysis of any software project: produces a structured report covering Overview,
  Architecture, Systems & Subsystems (with relationships, responsibilities, key modules,
  key functions, data flows), plus top-level Key Functions, Key Modules, and Data Flows.
  Trigger this skill whenever the user says things like "explore this project", "analyze the
  codebase", "give me an overview of this repo", "explain the architecture", "document this
  project", "what does this code do", "map out the system", or uploads/points to a project
  directory and wants to understand it. Also trigger when the user asks for any subset of
  the report sections (e.g. "what are the key modules?", "explain the data flow").
---


# Project Explorer

---

## 1. Overview

One or two paragraphs: what this project does, who it's for, its primary value proposition,
and the technology stack in plain language. No bullet lists — flowing prose.

---

## 2. Architecture

Describe the high-level architectural style (monolith, microservices, event-driven, MVC,
layered, hexagonal, etc.) and why it fits this project. Include a simple ASCII diagram
or text description of the major layers/services and how they connect.

Example diagram style:

    ┌─────────────┐     HTTP      ┌──────────────┐     SQL    ┌──────────┐
    │  Web Client │ ──────────▶  │  API Server  │ ─────────▶ │  DB      │
    └─────────────┘             └──────────────┘            └──────────┘
                                        │ events
                                        ▼
                                ┌──────────────┐
                                │  Job Queue   │
                                └──────────────┘

---

## 3. Systems & Subsystems

For each major subsystem/module:

### 3.x <Subsystem Name>

**Responsibility**: One sentence on what this subsystem owns.

**Relationships**:
- Depends on: <other subsystems or external services>
- Consumed by: <what calls into this>

**Key Modules** (files/classes):
| Module | Path | Role |
|--------|------|------|
| `AuthService` | `src/auth/service.ts` | Validates tokens, manages sessions |

**Key Functions**:
| Function | Signature (simplified) | What it does |
|----------|------------------------|--------------|
| `verifyToken` | `(token: string) → User` | Decodes JWT, checks expiry |

**Data Flow**:
Describe how data enters, transforms, and exits this subsystem.
Use a short numbered sequence or mini-diagram.

---

## 4. Key Modules (Project-wide)

Top 5–10 most important files/classes across the whole project.

| Module | Path | Why It Matters |
|--------|------|----------------|
| ... | ... | ... |

---

## 5. Key Functions (Project-wide)

Top 5–10 most important functions/methods across the whole project.

| Function | Location | Role |
|----------|----------|------|
| ... | ... | ... |

---

## 6. Data Flows

Describe 2–4 of the most important end-to-end data flows (e.g. "User Login", "Order
Checkout", "Background Sync"). For each:

### 6.x <Flow Name>

1. Step one — `module.function()` does X
2. Step two — data reaches Y via Z mechanism
3. ...

Include a simple sequence diagram in ASCII if it adds clarity.
# TaskFlow OS

> A full-stack automation platform that allows teams to connect APIs, trigger actions, and visualize automated processes — built with **React.js (Vite)**, **Node.js (GraphQL)**, and a **custom automation core**.

---

## Overview

TaskFlow OS merges three key capabilities:

- **Flow Building:** A drag-and-drop UI to design automations using triggers, conditions, and actions.  
- **Integration Layer:** A plug-in system for connecting external APIs (Slack, GitHub, Gmail, etc.).  
- **Execution Engine:** A backend service that runs flows, handles webhooks, and logs results.  

The platform is designed for modularity, extensibility, and later AI-powered flow generation.

---

## 🧩 Architecture Summary

| Layer | Technology | Responsibility |
|:------|:------------|:---------------|
| **Frontend** | React 18 + Vite + TailwindCSS + React Flow + Shadcn/UI | Interactive dashboard, flow builder, analytics. |
| **Backend API** | Node.js + Apollo GraphQL Server | CRUD for flows/runs, authentication, real-time subscriptions. |
| **Database** | PostgreSQL (via Prisma) / SQLite (dev) | Stores users, flows, runs, connectors, logs. |
| **Automation Core** | Custom TypeScript runner (+ n8n SDK optional) | Executes nodes, manages triggers/queues/actions. |
| **Auth** | JWT / Passport.js / Custom middleware | Session management and workspace permissions. |
| **Realtime** | WebSockets / GraphQL Subscriptions | Pushes live run status and flow edits to clients. |
| **AI Layer (optional)** | LangChain.js + LLM API | Generates or optimizes flows from natural language. |
| **Infra** | Docker + Render/Fly.io + Vercel (frontend) | Deployment and CI/CD management. |

---

## Module Responsibilities

### 1. Frontend (`/client`)

**Purpose:** Provide an interactive client interface for flow management.

**Key Directories:**
client/
├── src/
│ ├── components/ # Reusable UI elements (modals, buttons, forms)
│ ├── features/ # Feature modules (flows, dashboard, analytics)
│ ├── pages/ # Routed pages (Home, FlowBuilder, Runs)
│ ├── store/ # Zustand/Redux global state
│ ├── graphql/ # Apollo Client hooks and queries
│ └── utils/ # Helper functions and config

markdown
Copy code

**Responsibilities:**
- Render dashboard, flow editor, and analytics views.  
- Connect to GraphQL API via Apollo Client.  
- Manage drag-and-drop canvas using **React Flow**.  
- Handle JWT authentication and persist session in localStorage.  
- Subscribe to WebSocket events for live execution updates.  

---

### 2. Backend (`/server`)

**Purpose:** Expose a GraphQL API, manage users and workspaces, and coordinate the automation engine.

**Key Directories:**
server/
├── src/
│ ├── index.ts # Entry point
│ ├── graphql/ # TypeDefs and resolvers
│ ├── services/ # Business logic (flows, runs, users)
│ ├── models/ # Prisma schema models
│ ├── queue/ # BullMQ or in-memory job queues
│ ├── connectors/ # External API adapters
│ └── utils/ # Logger, config, error handlers

markdown
Copy code

**Responsibilities:**
- Authenticate users (JWT verification).  
- Serve GraphQL queries, mutations, and subscriptions.  
- Persist and retrieve flow configurations.  
- Dispatch automation jobs to the execution engine.  
- Log and expose flow run results.  

---

### 3. Database (`/prisma`)

**Purpose:** Persist all application entities.  

**Core Tables:**
- **User** – credentials, role, workspace mapping.  
- **Workspace** – collection of users and flows.  
- **Flow** – serialized node/edge configuration.  
- **FlowRun** – execution metadata and logs.  
- **Connector** – registered third-party integrations.  
- **AuditLog** – user actions and system events.  

---

### 4. Automation Core (`/automation`)

**Purpose:** Execute flows and manage connectors or triggers.

**Structure:**
automation/
├── triggers/ # Webhook and polling triggers
├── actions/ # Built-in and external connector actions
├── runner.ts # Sequential/parallel node execution
└── scheduler.ts # Cron or timed executions

yaml
Copy code

**Responsibilities:**
- Register and activate triggers.  
- Execute node graphs, resolve dependencies.  
- Push run-status events to backend via message bus.  
- Retry failed nodes and maintain idempotency.  

---

### 5. Integration SDK (`/connectors`)

**Purpose:** Provide a standardized interface for third-party integrations.

**Pattern:**
```ts
export interface Connector {
  name: string;
  triggers: Trigger[];
  actions: Action[];
}
```
## ⚙️ How Components Interact

1. **User Action:**  
   The user logs in and creates/edits a flow via the React UI.

2. **GraphQL API:**  
   The frontend sends queries/mutations to store or fetch flows.

3. **Automation Core:**  
   When a trigger fires, the backend dispatches a job to the runner.

4. **Execution:**  
   The engine executes nodes sequentially or in parallel and emits progress over WebSocket.

5. **Database:**  
   Results, errors, and logs are persisted in `FlowRun`.

6. **Frontend:**  
   Real-time updates refresh the dashboard and run details view.

---

## 🔐 Authentication

- **JWT tokens** include `userId`, `workspaceId`, and `role`.  
- **Express middleware** validates JWTs before resolver access.  
- **Apollo context** carries user info through each request.  

---

## 🧰 Developer Notes

- **Environment:** Node 18+, PNPM or Yarn workspace.  
- **Code Style:** ESLint + Prettier with TypeScript.  
- **Testing:** Jest / Vitest (unit) + Playwright (e2e).  
- **Logging:** Pino or Winston (JSON structured logs).  
- **Error Handling:** Centralized GraphQL middleware and automation error mapper.  

---

## 🧭 Extension Ideas

- Template marketplace for reusable flow blueprints.  
- Role-based dashboards (Admin vs Member).  
- AI Copilot for flow generation or debugging.  
- Public GraphQL Playground for developers.  
- Visual timeline of executions and audit events.  

---

## 🏁 Summary

**TaskFlow OS (React Edition)** unifies automation, collaboration, and extensibility into a single modern stack.  
By building it, you’ll master **React.js**, **GraphQL**, **API integration**, **queue-based automation**, and **AI-driven workflow design** —  
core technologies driving the next generation of developer-focused automation platforms.

---

## 🏁 Legacy Summary (Next.js Version)

**TaskFlow OS** (original Next.js concept) unified automation, collaboration, and extensibility into a single modern stack.  
By working on it, you’d gain hands-on mastery in **Next.js 14**, **GraphQL**, **API integration**, **queue-based automation**, and **AI-assisted software design** —  
core technologies driving today’s tech market.

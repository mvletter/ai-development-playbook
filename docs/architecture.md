# Architecture

> How the system fits together. Read to understand where code belongs and how components interact.

## System Overview

**One-liner:** [What does this system do in one sentence?]

**Example:** "Task management API that syncs between web and mobile with offline-first architecture."

```
[ASCII diagram showing major components and their relationships]

Example:
┌─────────────┐
│   Frontend  │
└──────┬──────┘
       │
┌──────▼──────────┐
│  API Gateway    │
└──────┬──────────┘
       │
   ┌───┴────┐
   │        │
┌──▼───┐ ┌─▼────┐
│ Auth │ │ Task │
│Service│ │Service│
└──┬───┘ └─┬────┘
   │       │
   └───┬───┘
       │
  ┌────▼─────┐
  │ Database │
  └──────────┘
```

## Directory Structure

**Where does new code belong?**

```
project/
├── src/
│   ├── controllers/   # HTTP handling ONLY - thin layer, no business logic
│   ├── services/      # Business logic HERE - fat services
│   ├── models/        # Database access - queries and schema
│   ├── utils/         # Shared utilities - validation, formatting, etc.
│   └── middleware/    # Express middleware - auth, logging, error handling
├── tests/
│   ├── unit/         # Service layer tests
│   └── integration/  # API endpoint tests
├── docs/
│   ├── architecture.md  # This file - system overview
│   ├── database.md      # Schema, queries, migrations
│   ├── patterns.md      # Reusable solutions
│   └── pitfalls.md      # Common mistakes
└── public/           # Frontend assets (if applicable)
```

**Rule:** If unsure where code belongs, check component constraints below.

**For detailed documentation:**
- **Backend services** - See detailed service patterns in docs/patterns.md#service-*
- **Database schema** - See complete schema and migration guide in docs/database.md
- **API endpoints** - See endpoint specifications in docs/patterns.md#api-* or create docs/api-endpoints.md for large projects
- **Frontend architecture** - For complex SPAs, consider creating docs/frontend.md with component structure

## Key Components

### [Component A - e.g., "TaskService"]

**Purpose:** [One sentence - what it does]
**Location:** [Exact path - e.g., src/services/TaskService.js]
**Talks to:**
- ↓ **Calls:** [Other components this calls - e.g., Database, NotificationService]
- ↑ **Called by:** [What calls this - e.g., TaskController, CronJobs]
- ❌ **NEVER calls:** [Layering constraints - e.g., Controllers, Frontend]

**Constraints:**
- ❌ NO HTTP handling (that's controllers)
- ❌ NO direct database queries (use models layer)
- ✅ MUST validate inputs with guard clauses
- ✅ MUST use transactions for multi-step operations

**For implementation details:**
- Service patterns: docs/patterns.md#service-transaction
- Database queries: docs/database.md#[table-name]
- Common mistakes: docs/pitfalls.md#service-fat-service

### [Component B - e.g., "TaskController"]

**Purpose:** [One sentence - what it does]
**Location:** [Exact path - e.g., src/controllers/TaskController.js]
**Talks to:**
- ↓ **Calls:** [Services only - e.g., TaskService, AuthService]
- ↑ **Called by:** [Routes/Express - e.g., router.post('/tasks')]
- ❌ **NEVER calls:** [Models, Database directly]

**Constraints:**
- ❌ NO business logic (that's services)
- ❌ NO direct database access (use services)
- ✅ MUST validate HTTP inputs (req.body, req.params)
- ✅ MUST use ApiResponse.success/error for responses

**For implementation details:**
- Controller patterns: docs/patterns.md#api-controller-thin
- Request validation: docs/patterns.md#api-validation
- Common mistakes: docs/pitfalls.md#api-fat-controller

### [Component C - e.g., "Frontend SPA"]

**Purpose:** [One sentence - what it does]
**Location:** [e.g., public/js/]
**Talks to:**
- ↓ **Calls:** [Backend API endpoints via fetch/axios]
- ❌ **NEVER calls:** [Database, Services directly]

**Constraints:**
- ❌ NO sensitive business logic (backend only)
- ❌ NO direct database access (API only)
- ✅ MUST validate user input before sending to API
- ✅ MUST handle loading states and errors gracefully

**For implementation details:**
- If complex frontend (>10 components): Create docs/frontend.md
- API integration patterns: docs/patterns.md#api-client
- State management: docs/patterns.md#frontend-state (if applicable)

## Data Flow

**How requests flow through the system**

### [Flow 1: e.g., "Create Task Request"]

```
1. User submits form → Frontend validates input
2. POST /api/tasks → TaskController.create()
3. Controller extracts request data → calls TaskService.create(data)
4. Service validates business rules → checks user permissions
5. Service calls TaskModel.insert(data) → Database transaction
6. Service triggers NotificationService.notify() → Background job
7. Service returns task object → Controller
8. Controller returns ApiResponse.success() → Frontend
```

**Constraints enforced in this flow:**
- Input validation happens TWICE (frontend + service layer)
- Business logic ONLY in service, never in controller
- Database writes ALWAYS use transactions
- Errors propagate up (never swallowed with catch-all)
- Background jobs triggered AFTER database commit

**Common pitfalls:**
- Putting business logic in controller → See docs/pitfalls.md#api-fat-controller
- Missing transaction → See docs/pitfalls.md#database-no-transaction
- Swallowing errors → See docs/pitfalls.md#error-catch-all

### [Flow 2]

...

## External Integrations

### [Integration A - e.g., "Email Service (SendGrid)"]

**Purpose:** [Why we integrate - e.g., "Send transactional emails"]
**Auth:** [How we authenticate - e.g., "API key in environment variable SENDGRID_API_KEY"]
**Rate limits:** [Constraints - e.g., "100 emails/hour on free tier"]
**Location:** [Where integration code lives - e.g., src/integrations/EmailClient.js]
**Fallback:** [What happens if it fails - e.g., "Emails queued in database for retry"]

**Constraints:**
- ❌ NEVER call directly from controllers (use service layer)
- ✅ MUST handle rate limit errors gracefully
- ✅ MUST log all API calls for debugging

## System Boundaries

**What can access what?**

- **Public:** [Unauthenticated access - e.g., "Login, signup, public docs"]
- **Authenticated:** [Requires valid token - e.g., "Task CRUD, user profile"]
- **Admin:** [Requires admin role - e.g., "User management, system settings"]

**Constraint:** Admin endpoints MUST check role, not just authentication.

## Key Decisions

**Major architectural choices** (for full context, see docs/decisions.md)

- **[Decision 1]:** [Brief summary + why - e.g., "Service-oriented architecture - separates business logic from HTTP handling, enables testing"]
- **[Decision 2]:** [e.g., "JWT auth - stateless, works with mobile apps, 24h expiry"]

**Link format:** See docs/decisions.md#adr-001

## Performance Considerations

**What keeps this system fast**

- **Caching strategy:** [e.g., "Redis for user sessions (1h TTL), in-memory for config (24h TTL)"]
- **Database indexing:** [e.g., "All foreign keys indexed, user_id + created_at composite index for queries"]
- **Rate limiting:** [e.g., "100 req/min per user, 1000 req/min per IP"]

**See also:** docs/patterns.md for implementation patterns

## Monitoring & Observability

**How we debug in production**

- **Logs:** [Where and format - e.g., "Winston logger, JSON format, logs/app.log"]
- **Metrics:** [What we track - e.g., "Request duration, DB query time, error rates"]
- **Alerts:** [What triggers alerts - e.g., "Error rate >5%, response time >2s"]

## Common Questions

**Where does [X] belong?**

| Code Type | Belongs In | Why |
|-----------|------------|-----|
| Input validation | Service layer | Business rules, reusable across endpoints |
| HTTP status codes | Controllers | HTTP concerns stay in HTTP layer |
| Database queries | Models layer | Separation of concerns, easier testing |
| Business logic | Services | Core domain logic, framework-agnostic |
| Error handling | Everywhere | Validate early, log and re-throw, never swallow |

**What if I need to add [Y]?**

- **New API endpoint:** Controller (thin) → Service (fat) → Model
- **New external API:** Create integration client in src/integrations/
- **New database table:** Migration + model + update docs/database.md
- **New background job:** Create in src/jobs/, trigger from service layer

**See also:**
- **Complete implementation patterns**: docs/patterns.md
- **Common mistakes to avoid**: docs/pitfalls.md
- **Database schema and queries**: docs/database.md
- **Architectural decisions**: docs/decisions.md

**For large projects, consider creating:**
- docs/frontend.md - Frontend component structure, state management
- docs/backend.md - Detailed service layer documentation
- docs/api-endpoints.md - Complete API reference with examples
- docs/integrations.md - External API client implementations

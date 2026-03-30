---
document: constitution
project: "Familienzentrale"
version: 1.0.0
status: Draft
last-updated: "2026-03-29"
build-mode: Production
builder-tool: Replit
---

# Project Constitution

> This document is the architectural DNA of Familienzentrale.
> Every task, prompt, and line of generated code must respect
> these rules. If a rule is unclear, ask. If a rule conflicts
> with a task, the constitution wins.

---

## 1. Stack

### Core Stack

| Layer | Technology | Version | Notes |
|-------|-----------|---------|-------|
| Frontend | React | 18.x | SPA, tablet-first (768px primary) |
| UI Components | MUI (Material UI) | 7.x | All UI components — no alternatives |
| Animation | Framer Motion | 11.x | All transitions, micro-interactions, gamification animations |
| Styling | MUI Theme + sx prop | — | Theme tokens are the single source of truth |
| CSS Utility | Tailwind CSS | 3.x | Layout utilities only (flex, grid, spacing). NOT for colors, typography, or component styling |
| State management | TanStack Query | 5.x | Server state. No client-only state library |
| Routing | React Router | 7.x | — |
| Data fetching | fetch (native) | — | Via queryClient helper functions |
| Calendar UI | Mobiscroll | Custom build | Calendar views only — no alternative calendar libraries |
| Drag and drop | @dnd-kit | 6.x | Calendar item rescheduling |
| Forms | React Hook Form + Zod | — | All forms — no uncontrolled form state |
| i18n | react-i18next | 16.x | DE primary, EN secondary |
| Backend | Node.js + Express | 5.x | Single server process |
| Database | PostgreSQL | — | Via Drizzle ORM |
| ORM | Drizzle | 0.39.x | All DB access — no raw SQL |
| Validation | Zod | 3.x | Shared between client and server |
| Auth | express-session + bcrypt | — | Session-based, httpOnly cookies |
| Language | TypeScript | 5.6.x | Mandatory everywhere |

### Approved Libraries

- `date-fns` for date formatting and manipulation
- `zod` for validation schemas (shared client/server)
- `framer-motion` for all animation
- `recharts` for charts and data visualization
- `lucide-react` for icons (supplement to MUI icons)
- `react-i18next` + `i18next` for internationalization
- `@dnd-kit/core` + `@dnd-kit/sortable` for drag and drop

### Forbidden Libraries

- All `@radix-ui/*` packages — removed, replaced by MUI
- `shadcn/ui` components — removed, replaced by MUI
- `class-variance-authority` — removed with shadcn
- `cmdk` — removed with shadcn
- `next-themes` — not applicable (MUI ThemeProvider handles theming)
- `vaul` — removed with shadcn
- `wouter` — removed, using react-router-dom
- `moment.js` — replaced by date-fns
- `jQuery` — not compatible with React model
- Any alternative component library (Ant Design, Chakra, etc.)
- Any alternative calendar library (FullCalendar, react-big-calendar, etc.)
- Any alternative animation library (react-spring, GSAP, etc.)

### Tailwind Scope Restriction

Tailwind CSS is permitted ONLY for layout utilities:
- Flexbox: `flex`, `flex-col`, `items-center`, `justify-between`, etc.
- Grid: `grid`, `grid-cols-*`, `gap-*`
- Spacing: `p-*`, `m-*`, `space-*` (when not inside MUI sx)
- Display: `hidden`, `block`, `inline-flex`
- Position: `relative`, `absolute`, `fixed`, `sticky`

Tailwind is FORBIDDEN for:
- Colors (`text-red-500`, `bg-blue-100`) — use MUI theme palette
- Typography (`text-lg`, `font-bold`) — use MUI Typography component
- Borders and radius (`rounded-lg`, `border`) — use MUI theme shape
- Shadows (`shadow-md`) — use MUI theme shadows
- Component styling of any kind — use MUI sx prop or styled()

### New Dependency Gate

Before adding any library not listed in Approved Libraries:
1. State what the library does
2. State why an approved library or framework primitive cannot do it
3. Check the license is compatible with MIT
4. Wait for PM approval

---

## 2. Architecture

### Layer Model

The system uses a two-layer architecture:
1. **Frontend** — React SPA (tablet-first, responsive to phone and desktop)
2. **Backend** — Express API server + static file serving

**Absolute rules:**
- The frontend communicates exclusively with the Express API via `/api/*` routes
- The frontend never accesses the database directly
- All business logic lives in server-side service modules (not in route handlers)
- Route handlers validate input and delegate to service functions
- The shared schema (`shared/schema.ts`) is the single source of truth for types
- Zod schemas defined in shared/ are used for both client-side and server-side validation

### Folder Structure

```
project-root/
├─ .sdd/
│  ├─ constitution.md          # This file
│  ├─ agents.md                # Builder operational instructions
│  ├─ specs/                   # One spec per user journey
│  ├─ plans/                   # Plans for complex features
│  └─ tasks.md                 # Build task list
│
├─ docs/
│  ├─ design-constraints.md
│  ├─ design-tokens.md
│  ├─ api-contracts.md
│  ├─ state-model.md
│  └─ seed-data.md
│
├─ client/
│  ├─ src/
│  │  ├─ components/           # Reusable UI components (shared across pages)
│  │  │  ├─ common/            # Generic: buttons, cards, dialogs, form elements
│  │  │  ├─ calendar/          # Calendar-specific components
│  │  │  ├─ gamification/      # XP, streaks, badges, leaderboard components
│  │  │  └─ navigation/        # AppShell, bottom nav, sidebar
│  │  ├─ pages/                # Route-level page components (max 300 lines each)
│  │  │  ├─ parent/            # Parent/admin views
│  │  │  ├─ child/             # Child-specific views (dedicated UI)
│  │  │  └─ shared/            # Views used by both (login, settings)
│  │  ├─ hooks/                # Custom React hooks
│  │  ├─ services/             # API client functions (fetch wrappers)
│  │  ├─ types/                # TypeScript type definitions (client-only)
│  │  ├─ utils/                # Pure utility functions
│  │  ├─ i18n/                 # Translation files (de.json, en.json)
│  │  ├─ theme/                # MUI theme definition + token constants
│  │  └─ animations/           # Framer Motion animation presets
│  ├─ public/
│  └─ index.html
│
├─ server/
│  ├─ index.ts                 # Server entry point
│  ├─ routes.ts                # Route registration (thin — delegates to service modules)
│  ├─ middleware/               # Auth middleware, validation middleware
│  ├─ services/                # Business logic modules (one per domain)
│  │  ├─ auth.ts
│  │  ├─ families.ts
│  │  ├─ tasks.ts
│  │  ├─ routines.ts
│  │  ├─ events.ts
│  │  ├─ timeBlocks.ts
│  │  ├─ rewards.ts
│  │  ├─ gamification.ts       # XP, streaks, levels, badges, challenges
│  │  ├─ calendar-sync.ts      # Google/Apple Calendar integration
│  │  ├─ care-share.ts
│  │  └─ notifications.ts
│  ├─ migrations/              # Numbered SQL migrations
│  └─ db.ts                    # Database connection
│
├─ shared/
│  └─ schema.ts                # Drizzle schemas + Zod types (single source of truth)
│
├─ package.json
├─ tsconfig.json
├─ vite.config.ts
└─ tailwind.config.ts
```

**Rules:**
- Page components must not exceed 300 lines. Extract sub-components.
- New components go in `client/src/components/[domain]/`
- New pages go in `client/src/pages/[role]/`
- API client functions go in `client/src/services/`
- Server business logic goes in `server/services/`
- Server route handlers go in `server/routes.ts` (or split by domain if routes.ts exceeds 500 lines)
- Shared types go in `shared/schema.ts`
- Do not create new top-level directories without PM approval

### Auth Model

- Session-based authentication via `express-session`
- Adult login: email + password (bcrypt hashed, 12 rounds)
- Child login: username + PIN (bcrypt hashed)
- Session stored server-side, httpOnly cookie sent to client
- Session expiry: 7 days
- Cookie settings: httpOnly, sameSite=lax, secure in production
- Role enforcement at API layer via middleware (not just UI hiding)
- Every protected endpoint checks `req.session.userId`
- Role-based access (adult vs child) checked per-endpoint
- Auth cannot be bypassed by URL manipulation
- Password hash never returned in API responses

---

## 3. Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Files (components) | PascalCase | `TaskCard.tsx` |
| Files (utilities) | camelCase | `formatDate.ts` |
| Files (hooks) | camelCase with `use` prefix | `useStreak.ts` |
| Files (services) | camelCase | `taskService.ts` |
| Files (types) | camelCase | `task.types.ts` |
| Directories | kebab-case | `calendar-sync/` |
| React components | PascalCase | `TaskCard` |
| Functions | camelCase | `getTaskById` |
| Variables | camelCase | `taskList` |
| Constants | UPPER_SNAKE_CASE | `MAX_XP_PER_TASK` |
| API routes | kebab-case, plural nouns | `/api/time-blocks` |
| Database tables | snake_case, plural | `time_blocks` |
| Database columns | snake_case | `created_by_user_id` |
| Enum values | snake_case | `task_completed` |
| i18n keys | dot-separated camelCase | `tasks.createNew` |
| CSS classes (Tailwind) | Tailwind utility classes | `flex items-center gap-4` |
| Branch names | type/description | `feat/child-daily-view` |
| Commit messages | conventional commits | `feat: add streak tracking` |

---

## 4. Code Standards

### General

- No `any` type in TypeScript — use explicit types or `unknown`
- No magic numbers or strings — use named constants
- No `console.log` in committed code — use the server `log()` function or remove before commit
- No commented-out code in committed files
- Every exported function has a JSDoc comment describing purpose, parameters, and return value
- Maximum file length: 300 lines for page components, 500 lines for service modules
- If a file exceeds the limit, extract logical units into separate files

### Frontend

- All API calls go through service functions in `/services` — no fetch calls in components
- No business logic in components — components render, hooks and services compute
- All forms use React Hook Form + Zod — no uncontrolled form state
- All lists that can be empty must handle the empty state
- All async operations must handle loading and error states
- All animations use Framer Motion — no CSS transitions for interactive elements
- All colors reference the MUI theme — no hardcoded hex values in components
- All text uses MUI Typography — no raw HTML text elements (`<p>`, `<span>`, `<h1>`)
- All spacing uses MUI theme spacing or Tailwind layout utilities — no arbitrary pixel values
- Every user-facing string must be wrapped in `t()` for i18n
- Components that exceed 150 lines must be split into sub-components

### Backend

- All API endpoints validate input with Zod before processing
- All database queries use Drizzle ORM — no raw SQL (except migrations)
- All error responses follow the standard shape: `{ error: { code: string, message: string } }`
- No business logic in route handlers — delegate to service functions in `server/services/`
- All endpoints that modify data require authentication
- All endpoints that access family data verify the user belongs to that family
- Role-based access checked per-endpoint (adult-only endpoints reject child sessions)

---

## 5. Testing Standards

### Production Requirements

- Manual journey verification against specs for all journeys
- All four states (loading, empty, error, success) verified per screen
- Zero console errors in normal operation
- All API endpoints tested with representative inputs (valid, invalid, edge cases)
- Auth enforcement tested: unauthenticated requests return 401, wrong-role requests return 403
- XP/level calculations verified with boundary values
- Streak logic verified across timezone boundaries
- Calendar sync tested with mock external calendar data
- Integration tests for all critical business logic (gamification engine, XP ledger, streak calculation)
- E2E tests for core user journeys (parent weekly overview, child daily view, task completion with XP)

---

## 6. Security Baselines

### All Builds

- No API keys, tokens, or secrets in frontend code or version control
- No raw user input rendered as HTML (XSS prevention)
- All form inputs have type constraints and length limits
- Error responses do not expose stack traces or internal structure
- No real PII in seed or demo data
- Session secret must be set via environment variable (not hardcoded)
- Child accounts cannot access adult-only endpoints
- Child accounts cannot modify family settings, rewards, or other members' data

### Production Requirements

- All inputs validated server-side (client-side validation is UX, not security)
- Parameterised queries throughout (Drizzle ORM enforces this)
- CORS explicitly configured — no wildcards in production
- Rate limiting on auth endpoints (login, signup)
- Session tokens httpOnly and secure in production
- Calendar sync tokens (OAuth) stored encrypted at rest
- Google/Apple OAuth tokens never exposed to the frontend
- XP ledger is append-only — no UPDATE or DELETE on points_ledger rows
- Password requirements enforced: minimum 8 characters, uppercase, lowercase, digit

---

## 7. Environment & Deployment

### Environments

| Environment | Purpose | Database | Auth |
|-------------|---------|----------|------|
| Development | Local / Replit dev | Replit PostgreSQL | Real session auth |
| Production | Live users | TBD (Replit PG for now) | Real session auth |

### Deployment Rules

- Environment variables documented in `.env.example` (never committed with real values)
- Database migrations tested in dev before production
- Rollback procedure: revert migration SQL available for every forward migration
- All environment-specific values come from `process.env` — no hardcoded URLs or credentials

---

## 8. Monitoring & Observability

- Error tracking tool: TBD (must be decided before Phase 5)
- Structured logging: JSON format via server `log()` function
- No PII in logs (no email addresses, no passwords, no session tokens)
- API response times logged for all `/api/*` requests (existing middleware)

---

## 9. Gates

### Simplicity Gate

Before introducing any new module, file, or abstraction layer:
**Question:** "Is this new module justified by the spec?"
If NO → do not create it. Use an existing module or inline the logic.

### Anti-Abstraction Gate

Before creating any custom implementation of common functionality:
**Question:** "Why not use MUI, Framer Motion, or an approved library?"
If no clear answer → use the framework primitive.

### New Dependency Gate

Before adding any new npm package:
1. Is this library in the Approved Libraries list? → Proceed
2. If not: what does it do that an approved library cannot?
3. Is the license compatible with MIT?
4. Wait for PM approval before installing

### Schema Change Gate

Before any database schema modification:
1. Is this change reflected in the spec?
2. Is the migration reversible?
3. Has the migration been tested in dev?
4. Wait for PM approval before applying

### File Size Gate

Before completing any task involving a page component:
**Question:** "Does this file exceed 300 lines?"
If YES → extract sub-components before marking the task complete.

---

## 10. Boundaries (Always Do / Ask First / Never Do)

### Always Do

- Follow naming conventions from Section 3
- Run TypeScript type check before committing
- Update tasks.md after completing a task
- Check this constitution before starting a task
- Handle loading, empty, error, and success states on every screen
- Use MUI components for all UI elements
- Use Framer Motion for all interactive animations
- Use the MUI theme for all colors, typography, and spacing
- Wrap all user-facing strings in `t()` for i18n
- Validate all API inputs with Zod
- Check auth and role on every protected endpoint
- Keep page components under 300 lines

### Ask First

- Change the database schema or entity model
- Add a new dependency not in the Approved Libraries list
- Refactor a core module or change file structure
- Change auth logic or session handling
- Add a new API endpoint not in the API contracts
- Modify a migration file
- Change the folder structure
- Add a new environment variable
- Change the MUI theme tokens

### Never Do

- Bypass auth checks or role enforcement
- Log sensitive data (PII, tokens, passwords)
- Delete migration files
- Introduce a new UI component library
- Use Radix, shadcn, or any removed library
- Disable TypeScript strict mode
- Hardcode environment-specific values
- Commit secrets or credentials to version control
- Use `any` type to bypass TypeScript checks
- Use Tailwind for colors, typography, or component styling
- Skip error/loading/empty states on any screen
- Put business logic in React components
- Put business logic in Express route handlers
- Use raw SQL outside of migration files
- Make XP ledger entries mutable (UPDATE/DELETE)
- Return password hashes in API responses
- Allow child accounts to access adult-only endpoints

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0.0 | 2026-03-29 | Initial constitution for v2 rebuild | PM + VEGA |

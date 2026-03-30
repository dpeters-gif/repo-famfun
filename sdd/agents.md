---
document: agents
project: "Familienzentrale"
version: 2.0.0
status: Active
last-updated: "2026-03-30"
builder-tool: Replit
---

# Agent Instructions

> You are an AI builder creating Familienzentrale from scratch.
> Before starting any task, read this file and `.sdd/constitution.md`.
> Follow the instructions exactly. If something is unclear, ask.
> If a task conflicts with the constitution, refuse and escalate.

---

## 1. Before Every Task

1. Read the task description in `.sdd/tasks.md`
2. Read the referenced spec in `.sdd/specs/[journey].md`
3. Check `.sdd/constitution.md` — does this task respect all rules?
4. If the task requires an Ask First action (see constitution Section 10), stop and request PM approval
5. Check the File Size Gate: will this file exceed 300 lines (pages) or 500 lines (services)?
6. Proceed only when all checks pass

---

## 2. Commands

| Action | Command |
|--------|---------|
| Install dependencies | `npm install` |
| Start dev server | `npm run dev` |
| Run TypeScript check | `npm run check` |
| Push DB schema changes | `npm run db:push` |
| Run migrations | `npx tsx server/migrate.ts` |
| Build for production | `npm run build` |
| Start production | `npm run start` |

---

## 3. Project Structure

> Create this exact structure. Do not deviate without PM approval.

```
project-root/
├─ .sdd/                        # SDD artifacts — do not modify without PM approval
│  ├─ constitution.md
│  ├─ agents.md                  # This file
│  ├─ specs/
│  ├─ plans/
│  └─ tasks.md
│
├─ docs/                         # Reference documents
│  ├─ design-constraints.md
│  ├─ design-tokens.md
│  ├─ api-contracts.md
│  └─ seed-data.md
│
├─ client/
│  ├─ src/
│  │  ├─ components/
│  │  │  ├─ common/              # Shared UI: buttons, cards, dialogs, form elements
│  │  │  ├─ calendar/            # Calendar views and calendar items
│  │  │  ├─ gamification/        # XP bars, streak counters, badges, leaderboard
│  │  │  └─ navigation/          # AppShell, BottomNav, Sidebar, AppBar
│  │  ├─ pages/
│  │  │  ├─ parent/              # Parent/admin pages
│  │  │  ├─ child/               # Child-specific pages (dedicated UI)
│  │  │  └─ shared/              # Login, signup, settings, accept-invite
│  │  ├─ hooks/                  # Custom React hooks
│  │  ├─ services/               # API client functions (fetch wrappers)
│  │  ├─ types/                  # Client-only TypeScript types
│  │  ├─ utils/                  # Pure utility functions
│  │  ├─ i18n/                   # Translation files (de.json, en.json, index.ts)
│  │  ├─ theme/                  # MUI theme definition + token constants
│  │  ├─ animations/             # Framer Motion animation presets
│  │  └─ main.tsx                # App entry point
│  ├─ public/
│  └─ index.html
│
├─ server/
│  ├─ index.ts                   # Server entry point
│  ├─ routes.ts                  # Route registration (thin — delegates to services)
│  ├─ db.ts                      # Database connection
│  ├─ migrate.ts                 # Migration runner
│  ├─ middleware/
│  │  ├─ auth.ts                 # requireAuth, requireAdult, requireChildPermission
│  │  └─ validation.ts           # Zod validation middleware
│  ├─ services/                  # Business logic (one file per domain)
│  │  ├─ auth.ts
│  │  ├─ families.ts
│  │  ├─ invites.ts
│  │  ├─ children.ts
│  │  ├─ tasks.ts
│  │  ├─ routines.ts
│  │  ├─ events.ts
│  │  ├─ timeBlocks.ts
│  │  ├─ rewards.ts
│  │  ├─ gamification.ts         # XP, streaks, levels, badges, challenges
│  │  ├─ calendarSync.ts         # Google/Apple Calendar integration
│  │  ├─ careShare.ts
│  │  └─ notifications.ts
│  └─ migrations/                # Numbered SQL migration files
│
├─ shared/
│  └─ schema.ts                  # Drizzle schemas + Zod types (single source of truth)
│
├─ package.json
├─ tsconfig.json
├─ vite.config.ts
└─ tailwind.config.ts
```

**Rules:**
- New components go in `client/src/components/[domain]/`
- New parent pages go in `client/src/pages/parent/`
- New child pages go in `client/src/pages/child/`
- New shared pages go in `client/src/pages/shared/`
- API client functions go in `client/src/services/`
- Server business logic goes in `server/services/`
- Server middleware goes in `server/middleware/`
- Animation presets go in `client/src/animations/`
- Shared types and schemas go in `shared/schema.ts`
- Translation files go in `client/src/i18n/`
- Do not create new top-level directories without PM approval

---

## 4. Code Style

### File Creation

- One component per file
- File name matches the default export name
- Component files use PascalCase: `TaskCard.tsx`
- Utility files use camelCase: `formatDate.ts`
- Hook files use camelCase with `use` prefix: `useStreak.ts`
- Service files use camelCase: `taskService.ts`
- Migration files use numbered prefix: `001_create_users.sql`

### Imports

Group imports in this order (blank line between groups):
1. React and React hooks
2. External libraries (MUI, Framer Motion, date-fns, etc.)
3. Internal components and hooks
4. Services and utilities
5. Types (use `import type` where possible)

### Components

- Functional components only — no class components
- Props interface defined above the component, named `[ComponentName]Props`
- Destructure props in the function signature
- No inline styles — use MUI `sx` prop or `styled()`
- All colors from MUI theme — no hardcoded hex values
- All text in `<Typography>` — no raw `<p>`, `<span>`, `<h1>`
- All interactive animations use Framer Motion `<motion.div>`
- All user-facing strings wrapped in `t()` for i18n

### Error Handling

- All async functions wrapped in try/catch
- API errors caught at the service layer and returned as typed error objects
- Components display error state from the state model — never raw error messages
- Use TanStack Query's `isError` / `error` for data fetching error states

---

## 5. Git Workflow

### Branch Naming

- `feat/[task-id]-[short-description]` for features
- `fix/[task-id]-[short-description]` for fixes
- `refactor/[task-id]-[short-description]` for refactors

### Commit Messages

- `feat: [description]` — new feature
- `fix: [description]` — bug fix
- `refactor: [description]` — code restructure
- `test: [description]` — adding or updating tests
- `docs: [description]` — documentation only
- `chore: [description]` — maintenance, dependencies

---

## 6. Testing Expectations

| Task Type | "Done" means |
|-----------|-------------|
| Setup | The command runs without errors, project compiles |
| Schema | Entity exists with all fields from the spec, migration runs clean |
| API | Endpoint responds with correct shape per API contract, errors handled |
| UI | Screen renders on tablet viewport (768px), all states handled (loading/empty/error/success) |
| Wiring | UI displays real data from the API, not mocked data |
| States | All four states verified manually at tablet viewport (768px) and phone (375px) |
| Animation | Framer Motion animation plays correctly, no jank on tablet |
| Gamification | XP awarded correctly, streak calculated correctly, level-up triggers |
| i18n | All strings translated in both DE and EN |
| Auth | Login, session, role enforcement working. Child cannot access adult endpoints |
| Migration | Migration applies cleanly, rollback SQL exists |

---

## 7. After Every Task

1. Run TypeScript check: `npm run check`
2. Verify zero console errors in browser (tablet viewport, 768px)
3. Verify the page/component does not exceed 300 lines
4. Update `.sdd/tasks.md`:
   - Mark the task as `[x]` (completed)
   - If a stub was introduced, add it to the Stub Register
   - If a new risk was identified, add it to the Risk Register
5. Commit with a conventional commit message referencing the task ID
6. If the task was the last one in a phase, notify the PM for review

---

## 8. When You're Stuck

1. Re-read the spec — does it answer your question?
2. Re-read the constitution — does it have a rule for this?
3. Re-read the API contracts — does the contract define the behaviour?
4. If still unclear: **stop and ask the PM.**

Format your question as:
- **Task:** [which task you're working on]
- **Question:** [what is unclear]
- **Options:** [2-3 concrete options you see]
- **Recommendation:** [which option you'd choose and why]

---

## 9. Boundaries (Quick Reference)

### Always Do (no approval needed)
- Follow naming conventions
- Run TypeScript check before commit
- Handle all four states on every screen
- Use MUI for all components, Framer Motion for all animations
- Use MUI theme for all colors and typography
- Wrap strings in `t()` for i18n
- Keep pages under 300 lines, services under 500 lines
- Validate inputs with Zod
- Check constitution before starting a task

### Ask First (PM approval required)
- Change database schema beyond what's in the spec
- Add new dependency not in Approved Libraries
- Add new API endpoint not in api-contracts.md
- Change auth logic
- Change folder structure
- Change MUI theme tokens

### Never Do (refuse and escalate)
- Bypass auth or role enforcement
- Log PII or secrets
- Delete migrations
- Introduce new UI library
- Use Radix/shadcn components
- Use Tailwind for colors or typography
- Use `any` type
- Skip error/loading/empty states
- Put business logic in components or route handlers
- Hardcode environment values
- Make XP ledger entries mutable

---

## 10. Replit-Specific Rules

- All services run on port 5000 (the only non-firewalled port)
- Vite dev server and Express share port 5000 in development
- Environment variables set via Replit Secrets (never in code)
- Database is Replit's built-in PostgreSQL
- File changes auto-reload in development
- Do not modify `.replit` configuration without PM approval

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0.0 | 2026-03-29 | Initial agents for v2 rebuild | PM + VEGA |
| 2.0.0 | 2026-03-30 | Rewritten for greenfield build — no existing codebase references | PM + VEGA |

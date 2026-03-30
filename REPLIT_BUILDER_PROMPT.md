# Familienzentrale v2 — Replit Builder Master Prompt (Greenfield v3)

You are building Familienzentrale from scratch — a tablet-first family operating system with Habitica/Duolingo-level gamification. There is NO existing codebase. This is a Production-grade greenfield build.

## CRITICAL: Read These Files Before EVERY Task

Before starting ANY task, you MUST read these files from the repo:

1. `.sdd/constitution.md` + `.sdd/constitution-amendment-v1.1.md` — Non-negotiable architectural rules. The amendment supersedes where it conflicts.
2. `.sdd/agents.md` — Your operational instructions, commands, folder structure.
3. `.sdd/tasks.md` (v3) — The ordered task list. 189 tasks, 10 waves. Work top-to-bottom. Never skip phases.
4. The specific spec file referenced in the task (e.g., `.sdd/specs/P-1-parent-weekly-overview.md`)
5. `docs/design-tokens.md` — All colors, spacing, typography, animation values. Plus new tokens from S-3-ENHANCED §4.
6. `docs/design-constraints.md` — All UI rules. Date pickers use MUI X, NOT Mobiscroll.
7. `docs/api-contracts.md` + `docs/api-contracts-addendum.md` — All endpoint definitions.
8. `.sdd/specs/S-3-ENHANCED-gamification-animation.md` — The gamification overhaul. Read this before ANY gamification task.

## Your Operating Rules

### Architecture Rules (from constitution + amendment v1.1)

- **MUI v7** is the ONLY component library. No Radix, no shadcn, no Chakra.
- **MUI X DatePicker / TimePicker** for all date/time inputs. NO Mobiscroll. No native HTML date inputs.
- **Custom calendar components** — WeekMatrix, DayView, MonthGrid are built with MUI Box/Grid + Framer Motion. NO third-party calendar rendering library.
- **Framer Motion** for ALL animations and transitions. No CSS transitions for interactive elements.
- **Web Audio API** for all gamification sounds. No external audio library. Procedural synthesis.
- **Tailwind CSS** is ONLY for layout utilities (flex, grid, spacing, position). NEVER for colors, typography, borders, shadows.
- **MUI theme tokens** are the single source of truth. No hardcoded hex colors.
- All text uses MUI `<Typography>`. No raw `<p>`, `<span>`, `<h1>`.
- All user-facing strings wrapped in `t()` for i18n (DE primary, EN secondary).
- All forms use React Hook Form + Zod.
- All API calls go through service functions in `client/src/services/`. Never fetch in components.
- All business logic in `server/services/`. Route handlers validate and delegate.
- Page components max 300 lines. Service modules max 500 lines.
- XP is NEVER spent. Gold is the spendable currency. They are separate.
- points_ledger is append-only. NO UPDATE or DELETE on ledger rows.

### Stack — Use Exactly This

| Layer | Technology | Notes |
|-------|-----------|-------|
| Frontend | React 18 + MUI v7 + Framer Motion 11 + TanStack Query v5 + React Router v7 | |
| Date/Time Pickers | @mui/x-date-pickers + date-fns adapter | Replaces Mobiscroll |
| Calendar Views | Custom MUI Box/Grid components | WeekMatrix, DayView, MonthGrid |
| Sound | Web Audio API (native) | Procedural generation, SoundEngine service |
| Backend | Node.js + Express 5 + Drizzle ORM + Zod | |
| Database | PostgreSQL (Replit built-in) | |
| Auth | express-session + bcrypt | Session-based, httpOnly cookies |
| PWA | manifest.json + service worker (workbox) | Installable, offline shell |
| i18n | react-i18next (de.json + en.json) | DE default |
| Styling | MUI theme + sx prop (primary), Tailwind layout only (secondary) | |
| Animation | Framer Motion exclusively | 25+ named presets |
| Forms | React Hook Form + Zod resolvers | |
| Charts | recharts | |
| Icons | lucide-react + MUI icons | |
| Drag/drop | @dnd-kit/core + @dnd-kit/sortable | |

### Before Adding ANY Dependency

If the task requires a new npm package not in constitution.md Approved Libraries or amendment v1.1:
1. State what it does
2. State why an approved library cannot do it
3. STOP and ask for PM approval. Do NOT install without approval.

## How To Execute Tasks

### Workflow Per Task

1. Read the task description in `.sdd/tasks.md`
2. Read the referenced spec(s)
3. Check constitution + amendment — does this respect all rules?
4. Implement following acceptance criteria exactly
5. Verify:
   - `npm run check` passes (TypeScript strict)
   - Zero console errors at 768px tablet AND 375px phone
   - File ≤ 300 lines (pages) or ≤ 500 lines (services)
   - All four states handled (loading, empty, error, success)
   - All strings in `t()`
   - All colors from MUI theme
   - Sound plays on gamification triggers (if applicable)
6. Mark task `[x]` in tasks.md
7. HANDOVER TEST / ARCHITECTURE CHECKPOINT: pause and verify ALL items
8. Commit: `feat: [TASK-ID] [description]`
9. Next task

### Task Execution Order — 10 Waves

**Phase A (Scaffolding):** Package.json, TypeScript, Vite, folder structure, MUI theme (full palette + MUI X overrides), animation presets (25+), SoundEngine service (14 procedural sounds), i18n, server entry, PWA manifest + service worker registration. NO feature code.

**Phase B (Schema):** All 26+ entities in shared/schema.ts. SQL migrations (6 files). Seed badge/avatar/boss/creature reference data. NO API or UI.

**Phase C (Auth):** Signup, login (email for adults, username+PIN for children), session middleware, role-based middleware. NO features.

**Phase D (APIs):** All endpoints per api-contracts.md + addendum. ~30 route groups. Task completion endpoint includes: XP + Gold calculation, drop evaluation (server-side random), streak freeze check, creature feeding, challenge progress, badge check — all in one DB transaction. NO UI.

**Phase E (Features) — 10 Waves:**

| Wave | Focus | Journeys |
|------|-------|----------|
| 1 | Parent screens (tablet-first) | P-1, P-2, P-3, P-5 |
| 2 | Child UI + enhanced animations + sound | C-1, C-2, C-3 |
| 3 | Gamification engine (gold, drops, freezes, bosses, creatures) | S-3 Enhanced, C-4, P-4 |
| 4 | Onboarding | S-1 |
| 5 | Family Board, Shopping List, Routine Flow Mode | P-13, P-11, P-8 |
| 6 | Smart Nudges, Photo Proof, Weekly Recap | P-9, C-5, P-10 |
| 7 | AI Suggestions, Avatar + Creatures | P-12, C-6 |
| 8 | Calendar Sync, Multi-Household | P-6, S-2, A-1/A-2 |
| 9 | Care-Share, PWA polish | P-7 |
| 10 | (Phase F-H) States audit, seed data, i18n, security, performance |

**Phase F (States):** Audit every screen for 4 states.
**Phase G (Polish):** Seed data, i18n, formatting, tokens.
**Phase H (Security):** Audits, LCP measurement, animation profiling, monitoring.

## Tablet-First Design

### This app lives on the kitchen iPad.

PRIMARY: **768px (iPad portrait)** and **1024px (iPad landscape)**.
Secondary: 375px–428px (phones).
Tertiary: 1280px+ (desktop — sidebar nav replaces bottom nav).

**Every screen:**
- Design for 768px FIRST. Test at 768px FIRST.
- Week matrix calendar is the DEFAULT on tablet. Day-tabs on phone only.
- Two-column card grids default. Single column on phone.
- Forms: centered dialogs (max 640px) on tablet, bottom sheets on phone.
- Gamification elements GENEROUS — readable at arm's length.
- Bottom nav (64px) on tablet AND phone. Desktop sidebar at ≥1280px.

## Key Design References

### Nordic Hearth + Duolingo/Habitica Palette

- Primary: `#5B7A6B` (sage green) — parent actions, navigation
- Secondary: `#C67B5C` (terracotta) — routines, warmth
- Background: `#FAF8F5` (creamy) — parent pages
- Child background: `#FFFBF0` (warmer)
- XP: `#FFB020` (gold) — XP awards, progress bars
- Gold currency: `#FFD700` — coin icon, spending UI
- Streak: `#FF6B35` (orange) — fire, counters
- Streak freeze: `#4FC3F7` (ice blue) — freeze crystals
- Level-up: `#7C4DFF` (purple) — celebrations
- Challenge: `#00BFA5` (teal) — progress
- Drops: `#E040FB` (magenta) — treasure chest glow
- Child CTA: `#FF6B35` (orange) — NOT green
- Boss HP: `#E53935` (red)

### Animation — The Dopamine Loop

The task completion sequence is the CORE of the product. Get this right:

```
T+0ms     Haptic (light) + Checkbox morph (spring, 300ms)
T+200ms   Sound: "ding" (±2 semitone random)
T+300ms   Card pulse + glow
T+350ms   "+15 XP" popIn (gold, float up) + "+3 🪙" popIn (slight delay)
T+500ms   First-of-day? → Streak flame pulse + streak chime
T+600ms   XP bar progressFill (500ms)
T+900ms   20% chance: Treasure chest bounceIn → open → item reveal
T+1100ms  Level-up? → Full-screen overlay + bounceIn number + confetti + fanfare
T+1200ms  Card strikethrough + fade + slide to completed
```

Every sound has a purpose. Every animation has a timing. Never skip, never overlap, always queue.

### Two Distinct UIs

**Parent/Adult:** Sage green, `#FAF8F5` background, 5-tab nav (Home, Calendar, Tasks, Rewards, Settings). Professional-warm. Week matrix default. Family Board section on Home.

**Child:** Orange CTAs, `#FFFBF0` background, 4-tab nav (My Day, Quests, Progress, Rewards). Game-like. Evolving flame, gold counter, creature companion, emotional avatar. Quest cards with XP badges. Family Board section on MyDay.

### Every Screen Must Have 4 States

1. **Loading:** Skeleton loaders matching content shape. Child: playful shapes.
2. **Empty:** Icon + heading + body + CTA. Never blank.
3. **Error:** Error icon + user-friendly message + retry. Child: friendly language.
4. **Success:** Toast (4s). Major: Framer Motion celebration. Level-up: full-screen.

### Gamification Engine — Key Rules

- **XP + Gold** are awarded on every task completion. XP = progress (never spent). Gold = currency (spendable).
- **Drops** have 20% chance on each completion. Evaluated server-side. 5 drop types.
- **Streak** is calculated from ledger (consecutive days). Freezes protect one missed day.
- **Flame evolves** through 6 visual tiers (gray ember → golden flame).
- **Milestones** at 3/7/14/30/50/100/365 days. Escalating celebrations.
- **Boss battles** show creature with HP bar. Tasks = damage. 4 visual damage stages.
- **Creatures** hatch from eggs (3 tasks), grow through feeding (5 tasks per feed stage).
- **Sound** plays on EVERY gamification moment. 14 sound tokens. Procedurally generated.
- **First-of-day bonus**: 1.5× XP on first task.
- **Avatar** has emotional states (happy, sleepy, excited, encouraging, celebrating).

## DO NOT (Global)

- DO NOT use Mobiscroll — removed from stack. Use MUI X DatePicker/TimePicker.
- DO NOT use any third-party calendar rendering library — custom build with MUI.
- DO NOT use any UI component library other than MUI v7.
- DO NOT use Tailwind for colors, typography, borders, or shadows.
- DO NOT use `any` type — explicit types or `unknown`.
- DO NOT put business logic in components or route handlers.
- DO NOT hardcode hex colors — use MUI theme.
- DO NOT use raw HTML text elements — use MUI Typography.
- DO NOT skip loading/empty/error states.
- DO NOT create files over 300 lines (pages) / 500 lines (services).
- DO NOT add dependencies without PM approval.
- DO NOT modify migration files after applying.
- DO NOT UPDATE or DELETE points_ledger rows. Append only.
- DO NOT allow Gold balance to go negative.
- DO NOT expose OAuth tokens to frontend.
- DO NOT return password hashes in API responses.
- DO NOT skip referenced spec when implementing a task.
- DO NOT implement features not in spec ("gold plating").
- DO NOT skip HANDOVER TEST or ARCHITECTURE CHECKPOINT.
- DO NOT design for phone first — tablet 768px is primary.
- DO NOT use console.log — use server log() or remove.
- DO NOT create custom abstractions when MUI/Framer primitives exist.
- DO NOT implement sound that can't be disabled.
- DO NOT use negative/shame language in child-facing UI. Ever.
- DO NOT evaluate drops client-side — server-side only.
- DO NOT show avatar crying or anger states — sleepy and encouraging are the most "negative" allowed.

## Start Now

Begin with **TASK-A001** in `.sdd/tasks.md`:

> Initialize monorepo — package.json with scripts, install all dependencies (including @mui/x-date-pickers, workbox).

Read the task. Read the constitution + amendment. Execute. Mark `[x]`. Commit. Move to TASK-A002.

Continue through all Phase A tasks (15 tasks) to get: compiling project, MUI theme with FULL palette (including gamification tokens), 25+ animation presets, procedural SoundEngine with 14 sounds, i18n, server, PWA manifest. Then Phase B schema. Then Phase C auth. Then Phase D APIs. Only then Wave 1 UI.

**Do not jump to UI before the foundation is solid.**

When you hit an ARCHITECTURE CHECKPOINT, stop and verify every listed item. Test at 768px.

When you hit a HANDOVER TEST, answer all 12 questions:
1. What are the inputs?
2. What are the outputs?
3. What state changes occur?
4. What does loading look like?
5. What does empty look like?
6. What does error look like?
7. What is frontend responsibility?
8. What is backend responsibility?
9. How is this authenticated?
10. What are the performance expectations?
11. How is this monitored?
12. What tests cover this feature?

If any answer is incomplete — fix it before marking done.

Go.

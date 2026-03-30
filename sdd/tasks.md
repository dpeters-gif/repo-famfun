---
document: tasks
project: "Familienzentrale"
version: 3.0.0
status: Active
build-mode: Production
spec-refs:
  - P-1 through P-13
  - C-1 through C-6
  - S-1 through S-3 (S-3 ENHANCED)
  - A-1/A-2
last-updated: "2026-03-30"
---

# Build Tasks — Greenfield v3

> Work through tasks top-to-bottom. Do not skip phases.
> Do not start a task without reading the referenced spec.
> Mark each task [x] when complete. Update stub and risk registers.
>
> This is a greenfield build. There is no existing code.
> Constitution amendment v1.1 is in effect (no Mobiscroll, PWA+Capacitor,
> sound system, gold currency, MUI X date pickers).
>
> 10 build waves. 22 journeys. ~190 tasks.

---

## Phase A: Project Scaffolding

- [ ] TASK-A001: Initialize monorepo — `package.json` with scripts (dev, build, start, check, db:push). Install all core deps: react@18, react-dom, react-router-dom@7, @mui/material@7, @mui/icons-material, @emotion/react, @emotion/styled, @mui/x-date-pickers, framer-motion@11, @tanstack/react-query@5, date-fns, zod, react-hook-form, @hookform/resolvers, react-i18next, i18next, i18next-browser-languagedetector, lucide-react, recharts, @dnd-kit/core, @dnd-kit/sortable. Dev deps: typescript@5.6, vite@7, @vitejs/plugin-react, tsx, esbuild, tailwindcss@3, autoprefixer, postcss, drizzle-kit. Server deps: express@5, express-session, bcrypt, pg, drizzle-orm, connect-pg-simple, memorystore. | spec: all | type: setup
- [ ] TASK-A002: Create `tsconfig.json` (strict, path aliases), `vite.config.ts` (React plugin, proxy, aliases), `tailwind.config.ts` (layout utilities ONLY). | spec: all | type: setup
- [ ] TASK-A003: Create full folder structure per agents.md §3 — all directories including `client/src/services/sound.ts` placeholder. | spec: all | type: setup
- [ ] TASK-A004: Create MUI theme in `client/src/theme/theme.ts` — full Nordic Hearth palette from design-tokens.md. Configure MUI X DatePicker theme overrides. | spec: design-tokens | type: setup
- [ ] TASK-A005: Create design token constants in `client/src/theme/tokens.ts` — all gamification colors including gold, drop, freeze, boss, pet tokens from S-3 ENHANCED. | spec: design-tokens, S-3-ENHANCED | type: setup
- [ ] TASK-A006: Create Framer Motion animation presets in `client/src/animations/presets.ts` — all presets: fadeIn, slideUp, slideInRight, popIn, bounceIn, checkmark, progressFill, streakFlame, shake, confetti, coinDrop, chestBounce, chestOpen, itemReveal, flameEvolve, bossHit, bossDefeat, petBounce, petFeed, numberRoll, streakFreeze. | spec: design-tokens §8, S-3-ENHANCED §4 | type: setup
- [ ] TASK-A007: Create SoundEngine service in `client/src/services/sound.ts` — Web Audio API procedural sound generation. Export named functions: playComplete(), playXPAward(), playGoldDrop(), playLevelUp(), playStreakFire(), playStreakMilestone(), playDropChest(), playDropOpen(), playBadgeEarn(), playBossHit(), playBossDefeat(), playError(), playFlowStep(), playFlowDone(). Each with ±2 semitone randomization. Master volume + mute controls. Total < 100KB. | spec: S-3-ENHANCED §3.2 | type: setup
- [ ] TASK-A008: Create i18n setup — `de.json` + `en.json` with section structure. DE default. | spec: all | type: setup
- [ ] TASK-A009: Create server entry point — Express 5, session, JSON parser, error handler, Vite dev middleware. Port 5000. | spec: constitution §2 | type: setup
- [ ] TASK-A010: Create migration runner in `server/migrate.ts`. | spec: all | type: setup
- [ ] TASK-A011: Create React entry point (`main.tsx`) + route definitions (`App.tsx`) — ThemeProvider, QueryClient, i18n, BrowserRouter. Routes: /login, /signup, /home, /calendar, /tasks, /rewards, /settings, /accept-invite, /create-family. | spec: all | type: setup
- [ ] TASK-A012: Create `client/src/lib/queryClient.ts` — TanStack Query config, apiRequest helper, getQueryFn factory. | spec: all | type: setup
- [ ] TASK-A013: Create `client/index.html` — DM Sans font from Google Fonts, viewport meta. | spec: all | type: setup
- [ ] TASK-A014: Create PWA manifest (`public/manifest.json`) — app name "Familienzentrale", icons, theme_color #5B7A6B, background_color #FAF8F5, display standalone, start_url /. Create basic service worker registration in main.tsx. | spec: constitution-amendment §2 | type: setup
- [ ] TASK-A015: Verify project compiles, starts, shows empty page at port 5000. Zero errors. | spec: all | type: verification

---

## Phase B: Schema & Migrations

### Core Entities (from original)

- [ ] TASK-B001: Create `shared/schema.ts` — User, Family, FamilyMember, FamilyInvite, ChildPermission. Zod schemas + TypeScript types exported. | spec: P-5, constitution §2 | type: schema
- [ ] TASK-B002: Create Task entity — id, familyId, title, description, icon, dueDate, startTime, endTime, priority, xpValue, photoRequired, status, visibility, assignedToUserId, createdByUserId, challengeId, timestamps. | spec: P-2, C-5 | type: schema
- [ ] TASK-B003: Create FamilyTag, TaskTag, TaskComment entities. | spec: P-2 | type: schema
- [ ] TASK-B004: Create Routine entity — including flowMode, flowTargetMinutes, flowStepOrder, photoRequired fields. Create RoutineTaskInstance. | spec: P-3, P-8 | type: schema
- [ ] TASK-B005: Create Event entity — with status field (active/pending for child events). | spec: P-2, C-1 | type: schema
- [ ] TASK-B006: Create TimeBlock entity. | spec: P-3 | type: schema

### Gamification Entities (enhanced)

- [ ] TASK-B007: Create PointsLedger entity — with goldAwarded column. Unique constraint (userId, taskId, reason). | spec: S-3-ENHANCED §2.1 | type: schema
- [ ] TASK-B008: Create Reward, RewardFulfillment entities. | spec: P-4 | type: schema
- [ ] TASK-B009: Create Streak, Level entities. | spec: S-3 | type: schema
- [ ] TASK-B010: Create Badge, UserBadge entities. | spec: S-3 | type: schema
- [ ] TASK-B011: Create Challenge, ChallengeProgress entities — with boss_battle type, bossCreature, bossHP fields. | spec: C-4, S-3-ENHANCED §2.6 | type: schema
- [ ] TASK-B012: Create FamilyQuest entity. | spec: C-4 | type: schema
- [ ] TASK-B013: Create LeaderboardSnapshot entity. | spec: C-3 | type: schema
- [ ] TASK-B014: Create DropEvent entity — id, userId, taskId, dropType, dropValue, createdAt. | spec: S-3-ENHANCED §2.2 | type: schema
- [ ] TASK-B015: Create StreakFreeze entity — id, userId, source (purchased/drop), activatedAt, createdAt. | spec: S-3-ENHANCED §2.3 | type: schema
- [ ] TASK-B016: Create CompanionCreature entity — id, userId, creatureType, stage (egg/baby/juvenile/adult), feedCount, hatchProgress, createdAt. | spec: S-3-ENHANCED §2.7 | type: schema

### New Feature Entities

- [ ] TASK-B017: Create NudgeRule entity. | spec: P-9 | type: schema
- [ ] TASK-B018: Create TaskCompletionPhoto entity. | spec: C-5 | type: schema
- [ ] TASK-B019: Create WeeklyRecap entity. | spec: P-10 | type: schema
- [ ] TASK-B020: Create ShoppingList, ShoppingItem entities. | spec: P-11 | type: schema
- [ ] TASK-B021: Create ChildAvatar, AvatarItem entities. | spec: C-6 | type: schema
- [ ] TASK-B022: Create BoardNote entity — id, familyId, authorUserId, text, imageUrl, expiresAt, createdAt. | spec: P-13 | type: schema
- [ ] TASK-B023: Create FamilyLinkRequest entity (multi-household). | spec: S-2 | type: schema

### Calendar Sync & Misc

- [ ] TASK-B024: Create CalendarConnection, ExternalCalendarEvent entities. | spec: P-6 | type: schema
- [ ] TASK-B025: Create FamilyProgress, FamilyReward, CareShareSnapshot entities. | spec: P-7, C-2 | type: schema
- [ ] TASK-B026: Create Notification entity. | spec: all | type: schema

### Migrations

- [ ] TASK-B027: Write migration `001_core.sql` — users, families, family_members, family_invites, child_permissions. | spec: all | type: migration
- [ ] TASK-B028: Write migration `002_tasks_routines.sql` — tasks (with photoRequired), family_tags, task_tags, task_comments, routines (with flow fields), routine_task_instances. | spec: P-2, P-3, P-8 | type: migration
- [ ] TASK-B029: Write migration `003_events_timeblocks.sql` — events (with status), time_blocks. | spec: P-2, P-3 | type: migration
- [ ] TASK-B030: Write migration `004_gamification.sql` — points_ledger (with goldAwarded), rewards, reward_fulfillments, streaks, levels, badges, user_badges, challenges (with boss fields), challenge_progress, family_quests, leaderboard_snapshots, drop_events, streak_freezes, companion_creatures. | spec: S-3-ENHANCED | type: migration
- [ ] TASK-B031: Write migration `005_new_features.sql` — nudge_rules, task_completion_photos, weekly_recaps, shopping_lists, shopping_items, child_avatars, avatar_items, board_notes, family_link_requests. | spec: P-8 through P-13, C-5, C-6, S-2 | type: migration
- [ ] TASK-B032: Write migration `006_sync_misc.sql` — calendar_connections, external_calendar_events, family_progress, family_rewards, care_share_snapshots, notifications. | spec: P-6, P-7 | type: migration
- [ ] TASK-B033: Seed badge reference data (9 badges + 3 streak milestone badges). Seed avatar items (14 Level-1 defaults + level/badge unlocks). Seed boss creature reference data (6 bosses). Seed starter creature data (6 types). | spec: S-3, S-3-ENHANCED, C-6 | type: seed
- [ ] TASK-B034: Apply all migrations. Verify all tables exist. | spec: all | type: migration
- [ ] TASK-B035: **ARCHITECTURE CHECKPOINT** — all entities match specs, migrations run cleanly, shared/schema.ts exports all types, TypeScript compiles. | spec: all | type: verification

---

## Phase C: Auth & Access Control

- [ ] TASK-C001: Implement auth service — createUser (email+password, bcrypt 12), createChildUser (username+PIN), authenticate, getUserById. | spec: constitution §2 | type: auth
- [ ] TASK-C002: Implement auth routes — signup, login (email OR username), logout, GET /me. | spec: api-contracts §1 | type: auth
- [ ] TASK-C003: Implement middleware — requireAuth, requireAdult, requireFamilyMember, requireChildPermission. | spec: A-1/A-2 | type: auth
- [ ] TASK-C004: Build login page — email+password (adults), username+PIN (children), tab switch. MUI components, Zod validation. | spec: constitution §2 | type: ui
- [ ] TASK-C005: Build signup page — name, email, password with requirements. | spec: constitution §2 | type: ui
- [ ] TASK-C006: Implement useAuth hook — TanStack Query, /api/auth/me. | spec: all | type: auth
- [ ] TASK-C007: Implement protected routes — ProtectedRoute, OnboardingRoute, role-based routing. | spec: all | type: auth
- [ ] TASK-C008: Rate limiting — 10 attempts per IP per 15 min on auth endpoints. | spec: constitution §6 | type: auth
- [ ] TASK-C009: Auth integration tests. | spec: all | type: test
- [ ] TASK-C010: **ARCHITECTURE CHECKPOINT** — auth flow works, roles enforced, rate limiting active. | spec: all | type: verification

---

## Phase D: API Endpoints

### Family Management APIs
- [ ] TASK-D001: Family service — createFamily, getFamilyForUser. | spec: P-5 | type: api
- [ ] TASK-D002: Invite service — createInvite, acceptInvite. | spec: P-5 | type: api
- [ ] TASK-D003: Children service — createChild, getChildPermissions, updateChildPermissions (incl. canCreateEvents). | spec: P-5 | type: api
- [ ] TASK-D004: Register family routes. | spec: P-5, api-contracts | type: api

### Task & Routine APIs
- [ ] TASK-D005: Task service — CRUD + completeTask (with gold calculation, drop evaluation, streak freeze check, creature feeding). | spec: P-2, S-3-ENHANCED | type: api
- [ ] TASK-D006: Routine service — CRUD + generateRoutineTasksForFamily (idempotent, 28-day window) + flow mode fields. | spec: P-3, P-8 | type: api
- [ ] TASK-D007: Register task and routine routes. | spec: api-contracts | type: api

### Gamification APIs
- [ ] TASK-D008: Gamification profile endpoint — GET /api/gamification/profile/:userId (includes gold balance, streak freezes, active creature). | spec: S-3-ENHANCED, api-contracts §3 | type: api
- [ ] TASK-D009: Leaderboard endpoint — GET /api/gamification/leaderboard. | spec: C-3 | type: api
- [ ] TASK-D010: Streak history endpoint — GET /api/gamification/streak-history/:userId. | spec: C-3 | type: api
- [ ] TASK-D011: Gold spend endpoint — POST /api/gamification/spend-gold (streak freeze purchase, avatar item purchase). | spec: S-3-ENHANCED §2.1 | type: api
- [ ] TASK-D012: Drop evaluation logic in task completion service — server-side random, 20% base probability, weighted distribution. | spec: S-3-ENHANCED §2.2 | type: api

### Challenge & Reward APIs
- [ ] TASK-D013: Challenge service — CRUD + boss battle type support. | spec: P-4, C-4, S-3-ENHANCED §2.6 | type: api
- [ ] TASK-D014: Reward service — CRUD + fulfillReward. | spec: P-4 | type: api

### New Feature APIs
- [ ] TASK-D015: Board note service — CRUD + image upload (compressed JPEG). | spec: P-13 | type: api
- [ ] TASK-D016: Shopping list service — CRUD + clear checked. | spec: P-11 | type: api
- [ ] TASK-D017: Nudge rule service — CRUD + trigger evaluation logic. | spec: P-9 | type: api
- [ ] TASK-D018: Photo proof — extend task completion endpoint with multipart upload. | spec: C-5 | type: api
- [ ] TASK-D019: Weekly recap service — generation logic + GET current/previous. | spec: P-10 | type: api
- [ ] TASK-D020: Avatar service — GET/PUT avatar, GET items with unlock filtering. | spec: C-6 | type: api
- [ ] TASK-D021: Creature service — GET creatures, feed logic, hatch logic. | spec: S-3-ENHANCED §2.7 | type: api
- [ ] TASK-D022: AI suggestion endpoint — GET /api/tasks/suggestions (Anthropic API call with anonymized history). | spec: P-12 | type: api
- [ ] TASK-D023: Flow mode endpoint — GET /api/routines/:id/flow-tasks. | spec: P-8 | type: api

### Calendar & Sync APIs
- [ ] TASK-D024: Calendar view endpoint — GET /api/calendar/view/family (time blocks, events, external, tasks, routines). | spec: P-1, api-contracts §4 | type: api
- [ ] TASK-D025: Calendar connection service — OAuth flow, sync, external events. | spec: P-6 | type: api
- [ ] TASK-D026: Event approval endpoints — approve, reject, list pending. | spec: C-1, api-contracts | type: api

### Multi-Household APIs
- [ ] TASK-D027: Family link service — link request, confirm (PIN), decline, unlink. GET /api/families/mine. | spec: S-2 | type: api

### Other APIs
- [ ] TASK-D028: Notification service — create, list, mark read. | spec: all | type: api
- [ ] TASK-D029: Care-Share endpoint — GET /api/care-share. Adults only. | spec: P-7 | type: api
- [ ] TASK-D030: Settings endpoint — sound toggle, volume, locale, nudge config. | spec: S-3-ENHANCED §3.2 | type: api

- [ ] TASK-D031: **ARCHITECTURE CHECKPOINT** — all endpoints match contracts, error shapes correct, auth enforced, gamification transaction wraps task+XP+gold+streak+drops+badges+creature, TypeScript compiles. | spec: all | type: verification

---

## Phase E: Core Feature Logic

### Wave 1: Parent Screens (P-1, P-2, P-3, P-5)

- [ ] TASK-E001: Build BottomNav.tsx — 5 items, Framer Motion scale on tap. | spec: design-constraints §6 | type: ui
- [ ] TASK-E002: Build AppBar.tsx — family name, notification bell, gold counter (children), user avatar. | spec: design-constraints §1, S-3-ENHANCED | type: ui
- [ ] TASK-E003: Build DesktopSidebar.tsx — collapsed 72px, expand 240px. ≥1280px only. | spec: design-constraints §1 | type: ui
- [ ] TASK-E004: Build FAB — quick-action menu: Task, Event, Routine, Board-Notiz. | spec: P-2, P-13 | type: ui
- [ ] TASK-E005: Build AppShell.tsx — role-aware (parent/child nav+bg). | spec: design-constraints §6, §10 | type: ui
- [ ] TASK-E006: Build NotificationMenu.tsx — dropdown list, mark read, empty state. | spec: all | type: ui
- [ ] TASK-E007: Build custom WeekMatrix.tsx — MUI Box/Grid (NO Mobiscroll). Members as columns, days as rows. Time blocks as background bands. Collapsed chip view. Today highlighted. | spec: P-1, AC-020 | type: ui
- [ ] TASK-E008: Build DayTabSelector + DayView — phone only (<768px). Swipe gestures. | spec: P-1, AC-021 | type: ui
- [ ] TASK-E009: Build TimeBlockBand, CalendarEventCard, CalendarTaskCard. | spec: P-1, AC-002, AC-003, AC-007, AC-008 | type: ui
- [ ] TASK-E010: Build MemberFilter.tsx — avatar chips. | spec: P-1, AC-004 | type: ui
- [ ] TASK-E011: Build parent Home page — compose WeekMatrix + MemberFilter + Board preview (P-13) + Quick Stats row. Wire to calendar view API. 4 states. | spec: P-1, P-13 | type: wiring
- [ ] TASK-E012: **HANDOVER TEST — P-1** | spec: P-1 | type: verification
- [ ] TASK-E013: Build IconPicker.tsx. | spec: P-2, AC-008 | type: ui
- [ ] TASK-E014: Build EventCreateForm — MUI DatePicker/TimePicker (NOT Mobiscroll). | spec: P-2, AC-002, AC-003 | type: ui
- [ ] TASK-E015: Build TaskCreateForm — with photoRequired toggle. | spec: P-2, C-5 | type: ui
- [ ] TASK-E016: Build ItemDetailView — edit/delete/save. | spec: P-2, AC-005 | type: ui
- [ ] TASK-E017: Wire create/edit/delete to APIs. Optimistic updates. | spec: P-2 | type: wiring
- [ ] TASK-E018: **HANDOVER TEST — P-2** | spec: P-2 | type: verification
- [ ] TASK-E019: Build TimeBlockCreateForm, TimeBlockList. | spec: P-3 | type: ui
- [ ] TASK-E020: Build RoutineCreateForm — with flow mode toggle + target duration + step ordering. | spec: P-3, P-8 | type: ui
- [ ] TASK-E021: Wire time blocks and routines to API. | spec: P-3 | type: wiring
- [ ] TASK-E022: **HANDOVER TEST — P-3** | spec: P-3 | type: verification
- [ ] TASK-E023: Build Settings page — family members, invites, child permissions, sound toggle + volume, locale. | spec: P-5, S-3-ENHANCED | type: ui
- [ ] TASK-E024: Build CreateFamily, AcceptInvite pages. | spec: P-5 | type: ui
- [ ] TASK-E025: Wire family management. | spec: P-5 | type: wiring
- [ ] TASK-E026: **HANDOVER TEST — P-5** | spec: P-5 | type: verification
- [ ] TASK-E027: **ARCHITECTURE CHECKPOINT — Wave 1** — all parent screens at 768px, week matrix renders, design tokens applied, 4 states handled. | spec: all | type: verification

### Wave 2: Child UI + Enhanced Animations + Sound (C-1, C-2, C-3)

- [ ] TASK-E028: Build ChildBottomNav — 4 items, orange accent. | spec: C-1, AC-023 | type: ui
- [ ] TASK-E029: Build ChildAppShell — warm bg, child nav, gold counter in bar. | spec: C-1, AC-007 | type: ui
- [ ] TASK-E030: Build GreetingSection — "Hallo [name]!", with emotional avatar state. | spec: C-1, AC-001, S-3-ENHANCED §3.5 | type: ui
- [ ] TASK-E031: Build StreakCard — evolving flame (6 visual tiers), streak count, at-risk shake, freeze indicator. | spec: C-1, S-3-ENHANCED §2.4 | type: ui
- [ ] TASK-E032: Build QuestCard — XP badge + Gold indicator, photo-required camera icon. | spec: C-1, AC-003, C-5 | type: ui
- [ ] TASK-E033: Build QuestList — two-column tablet, single phone. | spec: C-1, AC-009 | type: ui
- [ ] TASK-E034: Build XPLevelSection + GoldCounter — level badge, XP bar, gold coin display. | spec: C-1, AC-004, S-3-ENHANCED §2.1 | type: ui
- [ ] TASK-E035: Build ChallengePreview + RewardPreview + CreatureDisplay. | spec: C-1, AC-005, S-3-ENHANCED §2.7 | type: ui
- [ ] TASK-E036: Build child MyDay page — compose all sections + Board preview. Wire to APIs. 4 states. | spec: C-1, P-13 | type: wiring
- [ ] TASK-E037: **HANDOVER TEST — C-1** | spec: C-1 | type: verification
- [ ] TASK-E038: Build full completion animation choreography — checkmark → sound → XP pop + gold pop → bar fill → streak pulse → drop check → level check → card settle. Sequential queue. | spec: C-2, S-3-ENHANCED §3.1 | type: ui
- [ ] TASK-E039: Build LevelUpCelebration — full-screen overlay, bounceIn, confetti, sound. | spec: C-2, AC-004 | type: ui
- [ ] TASK-E040: Build DailyCompletionCelebration — all quests done confetti. | spec: C-2, AC-012 | type: ui
- [ ] TASK-E041: Build TreasureChestDrop — chest bounce in, open animation, item reveal, particle burst. | spec: S-3-ENHANCED §2.2 | type: ui
- [ ] TASK-E042: Build StreakMilestoneCelebration — tier-specific celebrations (7/14/30/50/100/365). | spec: S-3-ENHANCED §2.4 | type: ui
- [ ] TASK-E043: Wire task completion — full sequence including gold, drops, sound, haptics. | spec: C-2, S-3-ENHANCED | type: wiring
- [ ] TASK-E044: **HANDOVER TEST — C-2** | spec: C-2 | type: verification
- [ ] TASK-E045: Build LevelHero, StreakHistory (30-day heat map), BadgeCollection, Leaderboard. | spec: C-3 | type: ui
- [ ] TASK-E046: Build child Progress page — compose all, wire to gamification endpoints. | spec: C-3 | type: wiring
- [ ] TASK-E047: **HANDOVER TEST — C-3** | spec: C-3 | type: verification
- [ ] TASK-E048: **ARCHITECTURE CHECKPOINT — Wave 2** — child UI fully distinct, all animations smooth at 60fps on tablet, sound plays correctly, gold displays, drops trigger. | spec: all | type: verification

### Wave 3: Gamification Engine (S-3 Enhanced, C-4, P-4)

- [ ] TASK-E049: Build Gold spend UI — streak freeze purchase (10 Gold), avatar item purchase. In Settings > child profile. | spec: S-3-ENHANCED §2.1, §2.3 | type: ui
- [ ] TASK-E050: Build StreakFreeze indicator + purchase flow. | spec: S-3-ENHANCED §2.3 | type: ui
- [ ] TASK-E051: Build XP multiplier indicator — first-of-day bonus banner, active boost indicator on XP bar. | spec: S-3-ENHANCED §2.5 | type: ui
- [ ] TASK-E052: Build ChallengeDetail — with boss battle visualization (creature HP bar, 4 damage stages). | spec: C-4, S-3-ENHANCED §2.6 | type: ui
- [ ] TASK-E053: Build ChallengeCreateForm — including boss_battle type selector + creature picker. | spec: P-4 | type: ui
- [ ] TASK-E054: Build parent Rewards page — tabs: Rewards + Challenges. | spec: P-4 | type: ui
- [ ] TASK-E055: Build child Rewards page — reward cards with progress. | spec: P-4 | type: ui
- [ ] TASK-E056: Build BossDefeatCelebration — creature shatters, family celebration, bonus XP. | spec: S-3-ENHANCED §2.6, C-4 | type: ui
- [ ] TASK-E057: Build CompanionCreature component — egg incubation bar, 3 growth stages, feeding animation, idle bounce. | spec: S-3-ENHANCED §2.7 | type: ui
- [ ] TASK-E058: Implement leaderboard snapshot cron logic. | spec: S-3, AC-006 | type: logic
- [ ] TASK-E059: Wire all gamification UI to APIs. | spec: S-3-ENHANCED, C-4, P-4 | type: wiring
- [ ] TASK-E060: **HANDOVER TEST — C-4 + P-4 + S-3 Enhanced** | spec: all | type: verification
- [ ] TASK-E061: **ARCHITECTURE CHECKPOINT — Wave 3** — gold economy balanced, drops triggering, boss HP tracking, creatures growing, leaderboard snapshots generating. XP ledger immutable. | spec: all | type: verification

### Wave 4: Onboarding (S-1)

- [ ] TASK-E062: Build OnboardingWelcome — value proposition, "Los geht's" CTA. | spec: S-1, AC-001 | type: ui
- [ ] TASK-E063: Build OnboardingFamilyStep — family name input. | spec: S-1, AC-002 | type: ui
- [ ] TASK-E064: Build OnboardingChildrenStep — add children with username+PIN. | spec: S-1, AC-003 | type: ui
- [ ] TASK-E065: Build OnboardingSchoolStep — quick time block setup. | spec: S-1 | type: ui
- [ ] TASK-E066: Build OnboardingTaskStep — first task creation with suggestions. First creature egg awarded. | spec: S-1, S-3-ENHANCED §2.7 | type: ui
- [ ] TASK-E067: Build OnboardingComplete — mini celebration, calendar preview. | spec: S-1 | type: ui
- [ ] TASK-E068: Wire onboarding flow — step transitions, API calls, resume logic. | spec: S-1 | type: wiring
- [ ] TASK-E069: **HANDOVER TEST — S-1** | spec: S-1 | type: verification

### Wave 5: Family Board, Shopping List, Routine Flow Mode (P-13, P-11, P-8)

- [ ] TASK-E070: Build BoardNoteCard — author avatar, text (2-line truncate), image thumbnail, timestamp, expiry indicator. | spec: P-13, AC-006 | type: ui
- [ ] TASK-E071: Build BoardSection — for Home dashboard, 3 most recent notes, "+" button, "View all" link. | spec: P-13, AC-001 | type: ui
- [ ] TASK-E072: Build BoardFullView — scrollable list, expired toggle, image lightbox. | spec: P-13, AC-007 | type: ui
- [ ] TASK-E073: Build BoardNoteCreateForm — text area (500 chars), image upload, expiry date (MUI DatePicker). | spec: P-13, AC-003 | type: ui
- [ ] TASK-E074: Wire board to APIs. Integrate into parent Home + child MyDay. | spec: P-13 | type: wiring
- [ ] TASK-E075: **HANDOVER TEST — P-13** | spec: P-13 | type: verification
- [ ] TASK-E076: Build ShoppingListView — sticky add field, checklist, check/uncheck, clear checked. | spec: P-11, AC-001 to AC-005 | type: ui
- [ ] TASK-E077: Wire shopping list — optimistic updates, auto-retry. Accessible from Settings shortcut. | spec: P-11 | type: wiring
- [ ] TASK-E078: **HANDOVER TEST — P-11** | spec: P-11 | type: verification
- [ ] TASK-E079: Build FlowModeScreen — full-screen, single step, countdown timer, progress bar, step-complete sound, XP per step. | spec: P-8, AC-001 to AC-007 | type: ui
- [ ] TASK-E080: Build FlowModeComplete — total XP, total time, under-time bonus message. | spec: P-8, AC-006, AC-007 | type: ui
- [ ] TASK-E081: Wire flow mode — load routine tasks in order, completion API per step, exit confirmation. | spec: P-8 | type: wiring
- [ ] TASK-E082: **HANDOVER TEST — P-8** | spec: P-8 | type: verification

### Wave 6: Smart Nudges, Photo Proof, Weekly Recap (P-9, C-5, P-10)

- [ ] TASK-E083: Build NudgeConfigUI — per-child time pickers (1-3 times), enable/disable, parent alert toggle. In Settings. | spec: P-9, AC-010 to AC-013 | type: ui
- [ ] TASK-E084: Implement nudge trigger service — scheduled evaluation, context-aware message composition, duplicate suppression. | spec: P-9, AC-001 to AC-006 | type: logic
- [ ] TASK-E085: **HANDOVER TEST — P-9** | spec: P-9 | type: verification
- [ ] TASK-E086: Build camera capture UI — full-screen viewfinder, take/retake, gallery fallback. | spec: C-5, AC-003, AC-004 | type: ui
- [ ] TASK-E087: Wire photo proof into task completion — camera on photoRequired tasks, multipart upload, photo in task detail. | spec: C-5, AC-005, AC-006, AC-007 | type: wiring
- [ ] TASK-E088: **HANDOVER TEST — C-5** | spec: C-5 | type: verification
- [ ] TASK-E089: Build WeeklyRecapCard (Home preview) + WeeklyRecapDetail (full view). Per-member stats, week-over-week comparison. | spec: P-10, AC-001 to AC-006 | type: ui
- [ ] TASK-E090: Build child recap mini-card for MyDay. | spec: P-10, AC-005 | type: ui
- [ ] TASK-E091: Implement recap generation service — Monday 00:00 trigger, calculation from ledger. | spec: P-10, AC-001 | type: logic
- [ ] TASK-E092: **HANDOVER TEST — P-10** | spec: P-10 | type: verification

### Wave 7: AI Suggestions, Avatar, Creatures (P-12, C-6)

- [ ] TASK-E093: Build AI suggestion chips in TaskCreateForm — 3 suggestions, shimmer loading, tap to pre-fill. | spec: P-12, AC-001 to AC-003 | type: ui
- [ ] TASK-E094: Implement AI suggestion service — Anthropic API call, anonymized history, 1-hour cache, schema validation. | spec: P-12, AC-010 to AC-012 | type: logic
- [ ] TASK-E095: **HANDOVER TEST — P-12** | spec: P-12 | type: verification
- [ ] TASK-E096: Build AvatarEditor — category tabs, item grid, preview, locked items with requirements. | spec: C-6, AC-001 to AC-005 | type: ui
- [ ] TASK-E097: Build avatar placeholder SVG components — geometric style, 14 Level-1 items, level/badge unlock items. | spec: C-6, AC-010 to AC-012, constitution-amendment §5 | type: ui
- [ ] TASK-E098: Build LevelUpItemUnlock — "New item unlocked!" card after level-up. | spec: C-6, AC-004 | type: ui
- [ ] TASK-E099: Build creature placeholder SVGs — 6 types, 3 growth stages each (geometric animals). | spec: S-3-ENHANCED §2.7 | type: ui
- [ ] TASK-E100: Wire avatar + creatures to APIs. Display avatar on MyDay, nav, leaderboard. Display creature on MyDay. | spec: C-6, S-3-ENHANCED | type: wiring
- [ ] TASK-E101: **HANDOVER TEST — C-6 + creatures** | spec: C-6, S-3-ENHANCED §2.7 | type: verification

### Wave 8: Calendar Sync, Multi-Household (P-6, S-2)

- [ ] TASK-E102: Build calendar connection UI — Google OAuth flow, calendar list, sync direction, disconnect. | spec: P-6 | type: ui
- [ ] TASK-E103: Build ExternalEventCard — visually distinct, read-only, Google icon. | spec: P-6 | type: ui
- [ ] TASK-E104: Wire external events into WeekMatrix and DayView. | spec: P-6, P-1 | type: wiring
- [ ] TASK-E105: **HANDOVER TEST — P-6** | spec: P-6 | type: verification
- [ ] TASK-E106: Build household switcher in child app bar (visible when 2 families). | spec: S-2, AC-002 | type: ui
- [ ] TASK-E107: Build link-child flow — username input, pending confirmation, child PIN confirm, decline. | spec: S-2, AC-005 | type: ui
- [ ] TASK-E108: Wire multi-household — context switch, unified gamification, per-household data isolation. | spec: S-2 | type: wiring
- [ ] TASK-E109: **HANDOVER TEST — S-2** | spec: S-2 | type: verification
- [ ] TASK-E110: Build adult member permission enforcement — own-items-only editing, restricted settings. | spec: A-1/A-2 | type: ui
- [ ] TASK-E111: **HANDOVER TEST — A-1/A-2** | spec: A-1/A-2 | type: verification

### Wave 9: Care-Share, PWA Polish (P-7)

- [ ] TASK-E112: Build CareShareChart — donut chart (recharts), adult distribution, weekly/monthly toggle. | spec: P-7, AC-001, AC-002 | type: ui
- [ ] TASK-E113: Wire Care-Share — adults only redirect, card on Home. | spec: P-7, AC-003 | type: wiring
- [ ] TASK-E114: **HANDOVER TEST — P-7** | spec: P-7 | type: verification
- [ ] TASK-E115: Configure service worker with workbox — offline shell caching, API response caching strategy, cache-first for assets, network-first for API. | spec: constitution-amendment §2 | type: setup
- [ ] TASK-E116: Test PWA install flow on iOS Safari + Android Chrome. Verify standalone mode, splash screen, offline shell. | spec: constitution-amendment §2 | type: verification

---

## Phase F: UI States Audit

- [ ] TASK-F001: Audit all parent screens — loading/empty/error/success per design-constraints §7. | spec: all | type: states
- [ ] TASK-F002: Audit all child screens — playful skeletons, encouraging empty, child-friendly errors. | spec: C-1 through C-6 | type: states
- [ ] TASK-F003: Audit all forms — Zod validation, inline errors, submit spinner, data preservation. | spec: all | type: validation
- [ ] TASK-F004: Audit all animations — entrance animations, completion sequence, celebrations, no frame drops at 768px and 375px. | spec: all | type: polish
- [ ] TASK-F005: Audit sound system — all 14 sound tokens play correctly, volume control works, mute works, no audio delay. | spec: S-3-ENHANCED §3.2 | type: polish

---

## Phase G: Seed Data & Polish

- [ ] TASK-G001: Create comprehensive seed — Anna+Thomas (adults), Dennis+Lena (children). Time blocks, 15+ tasks, 5 routines, 8 events, 2 rewards per child, 1 boss battle challenge, XP ledger for Dennis Level 5 with 14-day streak, Lena Level 3. Badges, leaderboard snapshot. 3 board notes. 5 shopping items. Dennis has wolf companion (juvenile). Gold balances. 2 streak freezes for Dennis. | spec: all | type: seed
- [ ] TASK-G002: Complete all i18n — every string wrapped in t(). de.json + en.json fully populated. | spec: all | type: polish
- [ ] TASK-G003: Verify terminology consistency across all screens. | spec: all | type: polish
- [ ] TASK-G004: Verify date/number formatting per locale. | spec: all | type: polish
- [ ] TASK-G005: Verify navigation completeness — no dead ends, back works, direct URLs work. | spec: all | type: polish
- [ ] TASK-G006: Zero console errors at 768px + 375px. | spec: all | type: polish
- [ ] TASK-G007: Design token consistency — all colors from theme, all text in Typography, all spacing from theme. | spec: all | type: polish

---

## Phase H: Security & Performance

- [ ] TASK-H001: Security — no secrets in frontend, SESSION_SECRET from env, .env.example. | spec: constitution §6 | type: security
- [ ] TASK-H002: Security — all inputs Zod-validated server-side, no raw HTML rendering, no stack traces. | spec: constitution §6 | type: security
- [ ] TASK-H003: Security — CORS configured, rate limiting active. | spec: constitution §6 | type: security
- [ ] TASK-H004: Security — XP/Gold ledger immutability enforced, passwords never in responses, photo URLs family-scoped. | spec: S-3, C-5 | type: security
- [ ] TASK-H005: Security — child accounts cannot access adult endpoints. All boundaries tested. | spec: all | type: security
- [ ] TASK-H006: Performance — LCP < 2.5s on parent Home + child MyDay at throttled 4G. | spec: all | type: performance
- [ ] TASK-H007: Performance — zero layout shift after data loads. Skeletons match content. | spec: all | type: performance
- [ ] TASK-H008: Performance — test at scale: 50+ tasks, 10+ time blocks, 5 members, 100+ ledger entries, 50+ board notes. Add DB indexes. | spec: all | type: performance
- [ ] TASK-H009: Performance — animation profiling: all Framer Motion animations at 60fps on mid-range tablet. Sound playback < 10ms latency. | spec: S-3-ENHANCED | type: performance
- [ ] TASK-H010: Error monitoring setup — TBD tool, 500 error alerting. | spec: constitution §8 | type: monitoring

---

## Stub Register

| Stub ID | Location | Description | Removal Milestone | Status |
|---------|----------|-------------|-------------------|--------|
| STUB-001 | sound.ts | Procedural sounds — replace with professional audio post-launch | Post-launch | planned |
| STUB-002 | avatar SVGs | Geometric placeholder art — replace with illustrated art post-launch | Post-launch | planned |
| STUB-003 | creature SVGs | Geometric placeholder animals — replace with pixel/illustrated art post-launch | Post-launch | planned |
| STUB-004 | boss SVGs | Silhouette+color placeholders — replace with illustrated creatures post-launch | Post-launch | planned |

---

## Risk Register

| Risk ID | Description | Detection | Mitigation | Status |
|---------|-------------|-----------|------------|--------|
| RISK-001 | (REMOVED — Mobiscroll replaced with MUI X) | — | — | resolved |
| RISK-002 | Google Calendar OAuth requires verified app | OAuth consent screen warning in prod | Apply for Google verification at build start | open |
| RISK-003 | Framer Motion confetti/particles may lag on older tablets | Frame drops during celebrations | Profile on older iPad, reduce particle count | open |
| RISK-004 | XP/Gold ledger append-only may cause slow queries at scale | Slow leaderboard/level calcs | Add indexes on (userId, createdAt) early | open |
| RISK-005 | Express 5 middleware compatibility | Runtime errors | Pin version, test all middleware in Phase A | open |
| RISK-006 | Web Audio API inconsistency across browsers | Sounds don't play on some devices | Feature-detect AudioContext, silent fallback | open |
| RISK-007 | Anthropic API latency for AI suggestions | Slow form open | Cache suggestions 1hr per family, async fetch | open |
| RISK-008 | iOS PWA limitations (background sync, badging) | Nudge triggers missed | Document limitations, rely on in-app polling | open |
| RISK-009 | Gold economy inflation — too easy or too hard to earn | Children frustrated or bored | Monitor median gold balance per week, adjust ratios | open |

---

## Completion Summary

- Total tasks: 189
- Completed: 0
- Stubs remaining: 4 (all planned post-launch)
- Risks open: 8
- Specs covered: P-1 through P-13, C-1 through C-6, S-1 through S-3 (Enhanced), A-1/A-2
- Journeys: 22 + gamification enhancement
- Waves: 10
- All handover tests passed: [ ] no

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0.0 | 2026-03-29 | Initial 88 tasks, 14 journeys, 6 waves | PM + VEGA |
| 2.0.0 | 2026-03-29 | Added 8 new feature journeys, expanded to 9 waves | PM + VEGA |
| 3.0.0 | 2026-03-30 | Full regeneration: 189 tasks, 22 journeys, 10 waves. Mobiscroll removed, PWA added, sound system, gold currency, boss battles, creatures, procedural assets. | PM + VEGA |

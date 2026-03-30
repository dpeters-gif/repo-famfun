---
document: design-constraints
project: "Familienzentrale"
version: 1.0.0
status: Draft
last-updated: "2026-03-29"
depends-on:
  - constitution.md
  - design-tokens.md
---

# Design Constraints

> These rules apply to every screen the builder creates.
> They are verifiable: a reviewer can answer YES or NO
> to "does this screen follow the rule?"

---

## 1. Layout

- Tablet-first: all screens designed for 768px (iPad portrait) as primary viewport
- No horizontal scrollbars at any supported viewport (375px–1280px)
- Content area max-width: 960px on tablet, 1280px on desktop, centered
- Page content horizontal padding: 24px on tablet, 16px on phone, 32px on desktop
- Bottom navigation bar fixed at bottom, height 64px, visible on all authenticated screens (tablet and phone)
- Content area must have bottom padding of 80px (64px nav + 16px clearance) to prevent content hidden behind bottom nav
- On desktop (≥1280px): sidebar navigation replaces bottom nav, collapsed at 72px, expands to 240px on hover
- Top app bar height: 56px on phone, 64px on tablet and desktop
- No nested scrollable containers — only the page body scrolls

## 2. Data Display

- Task lists use card-based layout — one card per task, full width on mobile
- Each task card has a 3px left-border accent indicating priority (high=error, normal=primary, low=disabled)
- Routine-sourced tasks display a repeat icon (↻) and terracotta left-border accent
- Calendar week view: person-lane layout (members as columns, days as rows) on desktop; single-column day list on mobile
- Time blocks (school/work) render as colored background bands behind calendar events — not as event cards
- Empty states: centered icon (size-icon-xl) + descriptive text + primary action button. Never a blank area.
- All numbers formatted per locale (1.234 for DE, 1,234 for EN)
- All dates formatted as locale-appropriate (DD.MM.YYYY for DE, MM/DD/YYYY for EN)
- Leaderboard: ranked list with position number, avatar, name, XP count. Top 3 highlighted with gold/silver/bronze accent.

## 3. Actions & Controls

- One primary CTA per screen — visually dominant
- Primary actions: filled button with primary gradient background
- Secondary actions: outlined button
- Destructive actions: require confirmation dialog before execution
- All interactive elements have minimum touch target of 44×44px
- FAB (Floating Action Button) for primary create actions on mobile — bottom-right, 56px, above bottom nav
- Form submit buttons show loading spinner during submission and disable all form fields
- Completion actions (mark task done) use a dedicated tap target — not a small checkbox
- Task completion tap: checkbox expands to full-width row on tap, triggering Framer Motion checkmark animation

## 4. Status & Indicators

- Priority indicators use left-border accent (3px) on cards — not text labels alone
- Task status: open = unchecked circle, completed = filled check circle with success color
- Streak status: active = flame icon with streak color and glow, broken = gray flame, no streak = no icon
- XP progress: horizontal bar (12px height) with XP color fill and rounded ends
- Level display: circular badge with level number in extrabold weight
- Challenge progress: horizontal bar with challenge color fill, fraction text below (e.g., "3/10 tasks")
- Notification badge: red dot on bell icon, count shown

## 5. Forms

- Labels above inputs — not inline or floating
- Required fields marked with asterisk (*)
- Validation errors shown inline below the field in error color
- Form max-width: 480px — never full-width on tablet/desktop
- All inputs height: 48px (mobile touch-friendly)
- All date/time inputs use Mobiscroll pickers — no native date pickers
- Form sections separated by 24px vertical spacing
- Select dropdowns use MUI Select — no custom select implementations

## 6. Navigation & Routing

- Bottom navigation (mobile): 5 items maximum — Home, Calendar, Tasks, Rewards, Settings
- Active navigation item: filled icon + label in primary color. Inactive: outline icon + label in secondary text color.
- Child UI uses different bottom nav: My Day, Quests, Progress, Rewards (4 items)
- Page transitions use Framer Motion slideInRight (entering) and fade (exiting)
- Browser back button must work correctly on all routes
- Direct URL access must work or redirect to login if unauthenticated
- No breadcrumbs on mobile — rely on back arrows and navigation

## 7. States

- Every screen must handle four states: loading, empty, error, success
- Loading state: skeleton loaders matching the content layout shape — no spinners except inline button loading
- Empty state: centered illustration/icon + descriptive heading + body text + primary action CTA
- Error state: error icon + user-friendly message + retry button. No stack traces, no technical jargon.
- Success state: brief toast notification (Snackbar), auto-dismiss after 4 seconds. For major actions (task complete, level up): Framer Motion celebration animation.

## 8. Typography & Content

- No placeholder text in the final build — all labels, headings, and button text are final copy
- No Lorem ipsum, no TODO labels, no "Coming Soon"
- All user-facing strings wrapped in `t()` for i18n
- Truncate long text with ellipsis after 2 lines — no text overflow breaking layouts
- Section headers use weight 800 (extrabold) and "speak" to the user (e.g., "Deine Aufgaben heute" not "Aufgaben")
- Numbers that represent achievement (XP, level, streak count) use weight 800 and size-xxl or size-display

## 9. Gamification Visual Rules

- XP awards: pop-in animation with XP color, "+15 XP" floating text that rises and fades
- Streak counter: prominent position on child home screen, flame icon with animated glow when active
- Level-up: full-screen overlay with bounceIn animation, confetti particles, level number in display size
- Badge earned: popIn animation, badge appears with glow shadow, brief explanation text
- Leaderboard changes: position change indicated with up/down arrow and number of positions moved
- Challenge completion: celebration animation similar to level-up but with challenge color
- Quest progress: each contributing task lights up on the quest progress visual when completed
- All gamification numbers use tabular (monospace) figures so they don't shift layout when changing

## 10. Child UI Constraints

- Child view uses `color-child-bg` as page background (warmer tone)
- Child view uses larger touch targets: minimum 48×48px (vs 44×44px for adult)
- Child view uses `font-size-md` (17px) as base text size (vs 15px for adult)
- Child CTA buttons use `color-child-accent` (orange) instead of primary green
- Child view prioritizes visual elements over text — use icons, progress bars, and avatars prominently
- Child view does not show: Care-Share data, admin settings, family management, private events
- Child welcome screen shows: avatar/name greeting, today's streak, XP/level, today's tasks as "quests"
- Task cards in child view show point value prominently (XP badge on card) — not as small metadata

## 11. Calendar Visual Rules

- Time blocks (school/work) render as semi-transparent colored bands spanning the time range — behind all events
- Events render as solid cards/chips on top of time block bands
- Tasks with due time render as bordered cards at their time position
- All-day events render at the top of the day column (before timed items)
- Routine tasks visually distinguished by terracotta left-border and repeat icon
- Event cards show: title, time range, assigned member avatar(s)
- Calendar supports collapsed view (chips only, compact) and expanded view (full timeline with 30-min slots)
- Week view on tablet (768px+): full week matrix with person-lanes — this is the DEFAULT view
- Week view on phone (<768px): horizontal swipe between days, one day visible at a time with day selector tabs
- Month view: compact grid, days show dot indicators for item count, tap day to see day detail

## 12. Responsive Behaviour

- Primary target: 768px (iPad portrait) and 1024px (iPad landscape) — tablet is the home base
- Secondary target: 375px–428px (phones) — responsive adaptation for on-the-go use
- Tertiary target: 1280px+ (desktop) — bonus, not primary
- No features removed on smaller screens — only layout adapted
- Bottom nav on tablet and phone. Sidebar on desktop (≥1280px) only.
- Calendar: full week matrix is the DEFAULT on tablet (768px+). On phone (<768px): day-by-day swipe with day-selector tabs.
- Task list: two-column card grid on tablet, single-column on phone
- Forms: centered dialog (max 640px) on tablet, bottom sheet on phone (<768px)
- Child UI: quest cards in 2-column grid on tablet, single column on phone
- Gamification elements (XP bar, streak, level badge, leaderboard) sized for tablet — generous spacing, large badges, readable at arm's length (kitchen counter distance)

## 13. Decorative Elements

- No decorative elements without functional purpose
- No background images or gradients on content areas (except gamification celebrations which are temporary overlays)
- Icons used only when they add meaning — not for decoration
- Left-border accents (3px) on cards are functional — they communicate priority or type
- Whitespace preferred over dividers for section separation
- Gamification glow effects (shadows) are the exception to minimal decoration — they are part of the reward feedback loop

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0.0 | 2026-03-29 | Initial constraints for v2 — mobile-first + gamification + child UI rules | PM + VEGA |

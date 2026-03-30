---
document: constitution-amendment
project: "Familienzentrale"
version: 1.1.0
status: Draft
last-updated: "2026-03-30"
amends: constitution.md v1.0.0
---

# Constitution Amendment v1.1

> Apply these changes to `.sdd/constitution.md` before build begins.
> Amendments are additive unless marked REPLACE or REMOVE.

---

## Amendment 1: Remove Mobiscroll — Replace with MUI + Custom Calendar

**REMOVE** from Core Stack:

```
| Calendar UI | Mobiscroll | Custom build | Calendar views only |
```

**REPLACE** with:

```
| Calendar UI | MUI + custom components | — | Week matrix, day view, month grid — all built with MUI Box/Grid + Framer Motion. No third-party calendar library. |
```

**REMOVE** from Forbidden Libraries:

```
- Any alternative calendar library (FullCalendar, react-big-calendar, etc.)
```

**REPLACE** with:

```
- Any third-party calendar rendering library (FullCalendar, react-big-calendar, Mobiscroll, etc.) — calendar views are custom-built with MUI layout primitives
```

**ADD** to Approved Libraries:

```
- `@mui/x-date-pickers` for date/time picker inputs (replaces Mobiscroll pickers)
- `date-fns` adapter for MUI date pickers (`@mui/x-date-pickers/AdapterDateFns`)
```

**Rationale:** Mobiscroll requires a commercial license (RISK-001).
The week matrix, day view, and month grid are custom components
built with MUI Box/Grid and Framer Motion — no third-party calendar
renderer needed. Only the date/time input pickers require a library,
which MUI X provides free (MIT licensed for the base date picker).

**Impact on specs:** All references to "Mobiscroll picker" in P-2,
P-3, P-6, S-1, and design-constraints §5 are updated to
"MUI DatePicker / TimePicker."

**REMOVE** from Risk Register:

```
RISK-001 | Mobiscroll license needed for calendar views
```

---

## Amendment 2: Add PWA + Capacitor to Stack

**ADD** to Core Stack:

```
| PWA | Service Worker + Web App Manifest | — | Installable, offline shell, HTTPS required |
| Native wrapper | Capacitor | 6.x | iOS/Android wrapper — same React codebase. Added when app store distribution needed. |
```

**ADD** to Approved Libraries:

```
- `@capacitor/core` + `@capacitor/cli` for native app wrapping
- `@capacitor/push-notifications` for native push notifications
- `@capacitor/haptics` for device haptic feedback
- `@capacitor/camera` for enhanced camera access (photo proof)
- `workbox` for service worker generation (PWA caching)
```

**ADD** to Architecture section:

```
### Distribution Model

The app is distributed in three tiers:
1. **PWA (primary)** — accessible via URL, installable on home screen.
   Service worker provides offline shell and asset caching.
2. **Capacitor iOS** — same React app wrapped for App Store distribution.
   Adds native push, haptics, enhanced camera.
3. **Capacitor Android** — same React app wrapped for Play Store.

All three tiers share the same React codebase. No platform-specific
UI code. Capacitor plugins abstract native APIs behind a unified
JavaScript interface. Feature detection determines which capabilities
are available at runtime.
```

---

## Amendment 3: Add Sound System

**REMOVE** from C-2 Constraints:

```
- DO NOT implement sound effects — animation only for v2
```

**ADD** to Core Stack:

```
| Sound | Web Audio API (native) | — | Procedural sound generation. No external audio library. |
```

**ADD** to Code Standards — Frontend:

```
- All gamification sounds use the SoundEngine service in `client/src/services/sound.ts`
- Sound playback must never block or delay visual animations
- All sounds must be toggleable via Settings (default: ON at 70%)
- Sounds must respect device silent/mute mode
- Sounds use slight pitch randomization (±2 semitones) on repetitive triggers
- Total sound asset size must not exceed 100KB
- Procedural sounds preferred over audio file assets
```

---

## Amendment 4: Add Gold Currency

**ADD** to S-3 Gamification Engine — Core Mechanics:

```
### Gold Currency

- Gold is a spendable currency, separate from XP
- XP is never spent (progress measure). Gold is the spending currency.
- Gold ratio: 1 Gold per 5 XP earned (floor, minimum 1 for xp > 0)
- Gold can be spent on: streak freezes (10 Gold), avatar cosmetics,
  custom quest card backgrounds, parent-defined real rewards
- Gold balance displayed in child app bar
- Gold transactions recorded in points_ledger (new column: goldAwarded)
- Gold balance must never go negative
```

**UPDATE** S-3 Constraints — modify existing rule:

```
BEFORE: DO NOT implement mutable XP (no spending XP, no XP decay)
AFTER:  DO NOT implement mutable XP (no spending XP, no XP decay).
        Gold is a separate spendable currency — this rule does not
        apply to Gold.
```

---

## Amendment 5: Add Procedural Asset Strategy

**ADD** to new section — Asset Strategy:

```
### Visual Assets

- Avatar items (C-6): SVG components built inline in React.
  Placeholder style: geometric/minimal (circles, rectangles, simple
  shapes with the Nordic Hearth palette). Professional illustrated
  art replaces placeholders post-launch.
- Companion creatures (S-3): SVG components with 3 growth stages.
  Placeholder style: simple geometric animals (like Google's
  material design animal icons). Professional pixel art or
  illustrated art replaces placeholders post-launch.
- Boss battle creatures: SVG illustrations. Placeholder style:
  silhouette + color + health bar. Professional art post-launch.
- Empty state illustrations: Lucide icon compositions (approved
  library). No custom illustrations for v2 launch.

### Audio Assets

- All gamification sounds are procedurally generated using Web Audio
  API (OscillatorNode + GainNode + envelope shaping).
- SoundEngine service in `client/src/services/sound.ts` exports
  named functions matching sound tokens (e.g., playComplete(),
  playLevelUp(), playChestOpen()).
- Professional sound design replaces procedural sounds post-launch
  via swapping the SoundEngine implementation (same API surface).
```

---

## Amendment 6: Expand Journey Count and Wave Structure

**UPDATE** build waves from 6 to 10:

```
Wave 1: Parent screens (tablet-first, weekly overview, add/adjust, structure, family mgmt)
Wave 2: Child UI (daily view, task completion + enhanced animations + sound, progress)
Wave 3: Gamification engine (gold, drops, streaks, freezes, XP multipliers, challenges, rewards, boss battles)
Wave 4: Onboarding flow
Wave 5: New features — Family Board, Shopping List, Routine Flow Mode
Wave 6: New features — Smart Nudges, Photo Proof, Weekly Recap
Wave 7: New features — AI Suggestions, Child Avatar + Creatures
Wave 8: Calendar sync (Google OAuth), Multi-Household
Wave 9: Care-Share, PWA setup, Capacitor wrapper
Wave 10: States audit, seed data, i18n, security, performance
```

**UPDATE** total journey count:

```
Specs covered: P-1 through P-13, C-1 through C-6, S-1 through S-3, A-1/A-2
Total journeys: 22 + gamification enhancement
```

---

## Amendment 7: Update Date/Time Picker Constraint

**UPDATE** design-constraints.md §5 (Forms):

```
BEFORE: All date/time inputs use Mobiscroll pickers — no native date pickers
AFTER:  All date/time inputs use MUI X DatePicker / TimePicker with
        date-fns adapter — no native HTML date inputs, no third-party
        picker libraries
```

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.1.0 | 2026-03-30 | Mobiscroll removal, PWA+Capacitor, sound, gold, assets, expanded waves | PM + VEGA |

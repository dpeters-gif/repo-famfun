---
document: design-tokens
project: "Familienzentrale"
version: 1.0.0
status: Draft
last-updated: "2026-03-29"
depends-on:
  - constitution.md
---

# Design Tokens

> Design identity: **Nordic Hearth meets Duolingo**
> Warm, grounded Scandinavian palette (keeping the existing brand)
> combined with playful, game-like energy in interactions and
> the child UI. The palette stays cozy; the motion and personality
> become bold.

---

## 1. Colours

### Brand (Nordic Hearth — retained and refined)

| Token | Value | Usage |
|-------|-------|-------|
| `color-primary` | `#5B7A6B` | Primary actions, navigation active states, adult CTA |
| `color-primary-hover` | `#3D5A4A` | Hover state for primary elements |
| `color-primary-light` | `#EEF2EE` | Primary backgrounds, highlights, selected states |
| `color-primary-surface` | `rgba(91, 122, 107, 0.08)` | Subtle hover fills |
| `color-secondary` | `#C67B5C` | Routines, accents, warmth moments |
| `color-secondary-hover` | `#A65F3F` | Hover state for secondary elements |
| `color-secondary-light` | `rgba(198, 123, 92, 0.12)` | Secondary backgrounds |

### Gamification (NEW — Duolingo-inspired energy)

| Token | Value | Usage |
|-------|-------|-------|
| `color-xp` | `#FFB020` | XP awards, level progress bar fill, coin-like feel |
| `color-xp-light` | `rgba(255, 176, 32, 0.15)` | XP bar track, XP badge background |
| `color-streak` | `#FF6B35` | Streak fire icon, streak counter, streak cards |
| `color-streak-light` | `rgba(255, 107, 53, 0.12)` | Streak background accents |
| `color-level-up` | `#7C4DFF` | Level-up celebration, level badge, progression |
| `color-level-up-light` | `rgba(124, 77, 255, 0.12)` | Level background |
| `color-challenge` | `#00BFA5` | Active challenges, quest progress |
| `color-challenge-light` | `rgba(0, 191, 165, 0.12)` | Challenge card background |
| `color-badge-gold` | `#FFD700` | Gold badge fill |
| `color-badge-silver` | `#C0C0C0` | Silver badge fill |
| `color-badge-bronze` | `#CD7F32` | Bronze badge fill |
| `color-leaderboard-1st` | `#FFD700` | 1st place highlight |
| `color-leaderboard-2nd` | `#C0C0C0` | 2nd place highlight |
| `color-leaderboard-3rd` | `#CD7F32` | 3rd place highlight |

### Semantic

| Token | Value | Usage |
|-------|-------|-------|
| `color-success` | `#6B8F5B` | Task completion, available status |
| `color-success-light` | `rgba(107, 143, 91, 0.12)` | Success backgrounds |
| `color-warning` | `#D4943A` | Due soon, pending status |
| `color-warning-light` | `rgba(212, 148, 58, 0.12)` | Warning backgrounds |
| `color-error` | `#C25B4E` | Overdue, error states, high priority |
| `color-error-light` | `rgba(194, 91, 78, 0.12)` | Error backgrounds |
| `color-info` | `#5B8A9B` | Events, calendar items, informational |
| `color-info-light` | `rgba(91, 138, 155, 0.12)` | Info backgrounds |

### Time Block Colours

| Token | Value | Usage |
|-------|-------|-------|
| `color-block-school` | `rgba(91, 138, 155, 0.15)` | School time block band |
| `color-block-school-border` | `#5B8A9B` | School block left accent |
| `color-block-work` | `rgba(91, 122, 107, 0.12)` | Work time block band |
| `color-block-work-border` | `#5B7A6B` | Work block left accent |
| `color-block-unavailable` | `rgba(155, 168, 159, 0.12)` | Generic unavailability band |
| `color-block-unavailable-border` | `#9BA89F` | Unavailable block left accent |

### Priority Accents (left-border system)

| Token | Value | Usage |
|-------|-------|-------|
| `color-priority-high` | `#C25B4E` | Left border on high-priority cards |
| `color-priority-normal` | `#5B7A6B` | Left border on normal-priority cards |
| `color-priority-low` | `#9BA89F` | Left border on low-priority cards |

### Neutrals

| Token | Value | Usage |
|-------|-------|-------|
| `color-background` | `#FAF8F5` | Page background |
| `color-surface` | `#FEFDFB` | Card/panel backgrounds |
| `color-surface-subtle` | `#F3F0EB` | Subtle section backgrounds |
| `color-border` | `rgba(45, 58, 50, 0.08)` | Default borders, dividers |
| `color-border-hover` | `rgba(45, 58, 50, 0.12)` | Borders on hover |
| `color-border-strong` | `rgba(45, 58, 50, 0.20)` | Emphasis borders |
| `color-text-primary` | `#2D3A32` | Primary text |
| `color-text-secondary` | `#6B7B72` | Secondary text, labels |
| `color-text-disabled` | `#9BA89F` | Disabled text |
| `color-text-inverse` | `#FFFFFF` | Text on dark backgrounds |

### Child UI Palette Extension

| Token | Value | Usage |
|-------|-------|-------|
| `color-child-bg` | `#FFFBF0` | Child view background (warmer, more playful) |
| `color-child-surface` | `#FFFFFF` | Child card backgrounds |
| `color-child-accent` | `#FF6B35` | Child CTA buttons, active elements |
| `color-child-accent-hover` | `#E55A2B` | Child CTA hover |

---

## 2. Typography

### Font Family

| Token | Value |
|-------|-------|
| `font-family-base` | `'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif` |
| `font-family-display` | `'Inter', -apple-system, sans-serif` (weight 800 for display use) |
| `font-family-mono` | `'JetBrains Mono', monospace` |

### Font Sizes (mobile-first)

| Token | Value | Usage |
|-------|-------|-------|
| `font-size-xs` | `0.6875rem` (11px) | Captions, metadata, timestamps |
| `font-size-sm` | `0.8125rem` (13px) | Secondary text, chip labels |
| `font-size-base` | `0.9375rem` (15px) | Body text, form inputs (mobile-optimized) |
| `font-size-md` | `1.0625rem` (17px) | Subheadings, card titles |
| `font-size-lg` | `1.25rem` (20px) | Section headings |
| `font-size-xl` | `1.5rem` (24px) | Page titles |
| `font-size-xxl` | `2rem` (32px) | Hero numbers (XP count, level display) |
| `font-size-display` | `2.5rem` (40px) | Celebration numbers (level-up, milestone) |

### Font Weights

| Token | Value | Usage |
|-------|-------|-------|
| `font-weight-normal` | 400 | Body text |
| `font-weight-medium` | 500 | Labels, metadata |
| `font-weight-semibold` | 600 | Subheadings, button text, card titles |
| `font-weight-bold` | 700 | Page titles, emphasis |
| `font-weight-extrabold` | 800 | Display numbers (XP, levels), section headers that "speak" |

### Line Heights

| Token | Value | Usage |
|-------|-------|-------|
| `line-height-tight` | 1.2 | Headings, display numbers |
| `line-height-snug` | 1.35 | Subheadings, card titles |
| `line-height-base` | 1.5 | Body text |
| `line-height-relaxed` | 1.6 | Long-form content |

---

## 3. Spacing

> Base unit: 4px. Use multiples only.

| Token | Value | Usage |
|-------|-------|-------|
| `space-1` | 4px | Icon-to-text gap, tight gaps |
| `space-2` | 8px | Inline gaps, compact padding |
| `space-3` | 12px | Default input padding, chip padding |
| `space-4` | 16px | Standard gap, card inner padding (mobile) |
| `space-5` | 20px | Card inner padding (desktop), section spacing |
| `space-6` | 24px | Page content side padding (mobile) |
| `space-8` | 32px | Large section gaps |
| `space-10` | 40px | Major section dividers |
| `space-12` | 48px | Page-level vertical spacing |
| `space-16` | 64px | Bottom nav clearance |

---

## 4. Borders & Radii

| Token | Value | Usage |
|-------|-------|-------|
| `border-width` | 1px | Default border |
| `border-width-accent` | 3px | Left-border priority/type accent on cards |
| `border-radius-sm` | 8px | Chips, small buttons, inputs |
| `border-radius-md` | 12px | Cards, dialogs, standard containers |
| `border-radius-lg` | 16px | Large panels, bottom sheets |
| `border-radius-xl` | 20px | Modal dialogs, celebration overlays |
| `border-radius-full` | 9999px | Pills, avatars, circular buttons |

---

## 5. Shadows

| Token | Value | Usage |
|-------|-------|-------|
| `shadow-none` | none | Flat cards (default state) |
| `shadow-sm` | `0 1px 3px rgba(45, 58, 50, 0.06)` | Subtle lift (cards at rest) |
| `shadow-md` | `0 4px 12px rgba(45, 58, 50, 0.08)` | Cards on hover, dropdowns |
| `shadow-lg` | `0 8px 24px rgba(45, 58, 50, 0.12)` | Floating elements, bottom sheets |
| `shadow-xl` | `0 20px 40px rgba(45, 58, 50, 0.15)` | Modals, celebration overlays |
| `shadow-glow-xp` | `0 0 20px rgba(255, 176, 32, 0.3)` | XP award glow effect |
| `shadow-glow-streak` | `0 0 16px rgba(255, 107, 53, 0.25)` | Streak fire glow |
| `shadow-glow-levelup` | `0 0 30px rgba(124, 77, 255, 0.3)` | Level-up celebration glow |

---

## 6. Component Dimensions

| Token | Value | Usage |
|-------|-------|-------|
| `height-input` | 48px | Text inputs, selects (mobile-friendly) |
| `height-button` | 48px | Standard buttons (mobile touch target) |
| `height-button-sm` | 36px | Compact buttons (inside cards) |
| `height-button-lg` | 56px | Large CTA buttons |
| `height-bottom-nav` | 64px | Bottom navigation bar |
| `height-app-bar` | 56px | Top app bar (mobile) |
| `height-table-row` | 56px | List item rows (mobile touch target) |
| `height-calendar-chip` | 32px | Calendar event chips |
| `height-xp-bar` | 12px | XP progress bar height |
| `height-streak-badge` | 40px | Streak counter badge |
| `width-sidebar` | 240px | Desktop sidebar (expanded) |
| `width-sidebar-collapsed` | 72px | Desktop sidebar (collapsed) |
| `width-form-max` | 640px | Maximum form width (tablet-optimized) |
| `width-content-max` | 960px | Maximum content width on tablet |
| `size-icon-sm` | 16px | Small inline icons |
| `size-icon-md` | 20px | Standard icons |
| `size-icon-lg` | 24px | Large icons, navigation |
| `size-icon-xl` | 32px | Feature icons, gamification icons |
| `size-avatar` | 40px | User avatars |
| `size-avatar-sm` | 28px | Compact avatar (leaderboard rows) |
| `size-avatar-lg` | 56px | Profile avatar, child welcome screen |
| `size-badge` | 48px | Achievement badge icon |
| `size-touch-target` | 44px | Minimum touch/click target |

---

## 7. Breakpoints

| Token | Value | Target |
|-------|-------|--------|
| `breakpoint-xs` | 375px | Small phones (iPhone SE) |
| `breakpoint-sm` | 428px | Large phones (iPhone Pro Max) |
| `breakpoint-md` | 768px | Tablet portrait — PRIMARY TARGET |
| `breakpoint-lg` | 1024px | Tablet landscape — PRIMARY TARGET |
| `breakpoint-xl` | 1280px | Desktop (bonus) |

**Tablet-first rule:** All layouts designed for 768px–1024px first,
then adapted down to phones and up to desktop. The full week matrix,
two-column quest grids, and generous gamification elements are the
default — phones get a simplified single-column adaptation.

---

## 8. Animation & Transitions

### Durations

| Token | Value | Usage |
|-------|-------|-------|
| `duration-instant` | 100ms | Toggle, checkbox |
| `duration-fast` | 150ms | Hover states, button press feedback |
| `duration-base` | 200ms | Dropdown open, panel slide |
| `duration-moderate` | 300ms | Page transitions, card expand |
| `duration-slow` | 500ms | XP bar fill, celebration entrance |
| `duration-celebration` | 800ms | Level-up animation, reward unlock |

### Easing

| Token | Value | Usage |
|-------|-------|-------|
| `ease-out` | `cubic-bezier(0.0, 0.0, 0.2, 1)` | Elements entering view |
| `ease-in` | `cubic-bezier(0.4, 0.0, 1, 1)` | Elements leaving view |
| `ease-in-out` | `cubic-bezier(0.4, 0.0, 0.2, 1)` | State changes |
| `ease-bounce` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Playful bounce (XP pop, badge earn) |
| `ease-spring` | Framer Motion spring: `{ stiffness: 400, damping: 25 }` | Task completion snap |

### Framer Motion Presets (to be defined in `client/src/animations/`)

| Preset | Effect | Used for |
|--------|--------|----------|
| `fadeIn` | Opacity 0→1 | Page content entrance |
| `slideUp` | Y:20→0, opacity 0→1 | Cards entering, list items |
| `slideInRight` | X:100%→0 | Page transitions |
| `popIn` | Scale 0.8→1, opacity 0→1 | Badges, XP awards, notifications |
| `bounceIn` | Scale with overshoot | Level-up number, celebration elements |
| `confetti` | Particle burst | Major celebrations (level-up, quest complete) |
| `checkmark` | SVG path draw | Task completion checkmark animation |
| `progressFill` | Width 0→N% with easing | XP bar, challenge progress |
| `streakFlame` | Flicker + glow | Streak fire icon animation |
| `shake` | X oscillation | Error feedback, streak about to break warning |

---

## 9. Accessibility

| Token | Value | Standard |
|-------|-------|----------|
| `contrast-ratio-normal` | 4.5:1 | WCAG AA for normal text |
| `contrast-ratio-large` | 3:1 | WCAG AA for large text (18px+) |
| `focus-ring-width` | 2px | Focus indicator |
| `focus-ring-color` | `#5B7A6B` | Focus ring colour |
| `focus-ring-offset` | 2px | Space between element and focus ring |

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0.0 | 2026-03-29 | Initial tokens for v2 — Nordic Hearth + Duolingo gamification palette | PM + VEGA |

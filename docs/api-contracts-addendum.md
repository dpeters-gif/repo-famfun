---
document: api-contracts-addendum
project: "Familienzentrale"
version: 1.1.0
status: Draft
last-updated: "2026-03-30"
extends: api-contracts.md
---

# API Contracts — Addendum (New Features)

> Extends the base api-contracts.md with endpoints for 8 new features.
> Existing endpoints remain unchanged unless noted.

---

## 1. Routine Flow Mode (P-8)

### PATCH `/api/routines/:id` (EXTENDED)

Added flow mode fields.

**Additional request body fields:**
```typescript
{
  flowMode?: boolean;           // enable flow mode for this routine
  flowTargetMinutes?: number;   // target duration in minutes
  flowStepOrder?: string[];     // ordered task IDs for flow sequence
}
```

### GET `/api/routines/:id/flow-tasks`

Returns today's flow-mode tasks for a routine in order.

**Response (200):**
```typescript
{
  success: true,
  routine: { id: string; title: string; flowTargetMinutes: number },
  steps: {
    position: number;
    task: Task;
    completed: boolean;
  }[]
}
```

**Auth:** Required. Child or adult family member.

---

## 2. Smart Nudge Notifications (P-9)

### POST `/api/nudge-rules`

**Request body:**
```typescript
{
  familyId: string;
  childUserId: string;
  times: string[];              // ["15:00", "18:00"] — HH:mm format, max 3
  enabled: boolean;
  parentAlertEnabled: boolean;
  parentAlertTime?: string;     // HH:mm, default "19:00"
}
```

**Response (201):** `{ success: true, nudgeRule: NudgeRule }`
**Auth:** Adults only.

### GET `/api/nudge-rules`

**Query params:** `familyId` (required)
**Response (200):** `{ success: true, rules: NudgeRule[] }`
**Auth:** Adults only.

### PATCH `/api/nudge-rules/:id`

**Request body:** Partial NudgeRule fields.
**Response (200):** `{ success: true, nudgeRule: NudgeRule }`
**Auth:** Adults only.

### POST `/api/nudge-rules/trigger` (INTERNAL)

Triggered by a scheduled job. Evaluates all active nudge rules and
generates notifications. Not callable from the frontend.

---

## 3. Photo Proof of Completion (C-5)

### POST `/api/tasks/:taskId/complete` (EXTENDED)

When task has `photoRequired=true`, request must include photo.

**Request:** `multipart/form-data`
```
- photo: File (JPEG/PNG, max 5MB)
```

**Response (200):** Same as existing, plus:
```typescript
{
  // ...existing gamification response...
  photo?: {
    url: string;
    capturedAt?: string;        // from EXIF if available
  }
}
```

**Validation:** If `photoRequired=true` and no photo attached, return 400
with code `PHOTO_REQUIRED`.

### GET `/api/tasks/:taskId/photo`

**Response (200):** Returns the photo binary (JPEG).
**Auth:** Required. Family members only.

### POST `/api/tasks` (EXTENDED)

**Additional request body field:**
```typescript
{
  photoRequired?: boolean;      // default: false
}
```

### PATCH `/api/routines/:id` (EXTENDED)

**Additional request body field:**
```typescript
{
  photoRequired?: boolean;      // applies to all generated tasks
}
```

---

## 4. Weekly Family Recap (P-10)

### GET `/api/recaps/current`

**Query params:** `familyId` (required)

**Response (200):**
```typescript
{
  success: true,
  recap: {
    familyId: string;
    weekStart: string;          // YYYY-MM-DD (Monday)
    weekEnd: string;            // YYYY-MM-DD (Sunday)
    summary: {
      totalTasksCompleted: number;
      totalXPEarned: number;
      weekOverWeekChange: number; // percentage, e.g., 12 or -5
    };
    members: {
      userId: string;
      name: string;
      tasksCompleted: number;
      xpEarned: number;
      streakStatus: "active" | "started" | "broken" | "none";
      streakCount: number;
      badgesEarned: { id: string; name: string; icon: string }[];
    }[];
    challenges: {
      challengeId: string;
      title: string;
      weekStartCount: number;
      weekEndCount: number;
      targetCount: number;
    }[];
  } | null                      // null if recap not yet generated
}
```

**Auth:** Required. Adults see full recap. Children see own data only.

### GET `/api/recaps/previous`

Same shape as `/api/recaps/current` but for the previous week.

---

## 5. Shopping List (P-11)

### GET `/api/shopping-list`

**Query params:** `familyId` (required)

**Response (200):**
```typescript
{
  success: true,
  items: {
    id: string;
    name: string;
    checked: boolean;
    addedByUserId: string;
    addedByName: string;
    checkedByUserId?: string;
    checkedByName?: string;
    createdAt: string;
    checkedAt?: string;
  }[]
}
```

**Auth:** Required. All family members.

### POST `/api/shopping-list/items`

**Request body:**
```typescript
{
  familyId: string;
  name: string;                 // required, max 200 chars
}
```

**Response (201):** `{ success: true, item: ShoppingItem }`
**Auth:** Required. Adults always. Children if canCreateTasks.

### PATCH `/api/shopping-list/items/:id`

**Request body:**
```typescript
{
  checked?: boolean;
}
```

**Response (200):** `{ success: true, item: ShoppingItem }`
**Auth:** Required. All family members (including children for check).

### DELETE `/api/shopping-list/items/checked`

Deletes all checked items for the family.

**Query params:** `familyId` (required)
**Response (200):** `{ success: true, deletedCount: number }`
**Auth:** Adults only.

---

## 6. AI Task Suggestions (P-12)

### GET `/api/tasks/suggestions`

**Query params:** `familyId` (required)

**Response (200):**
```typescript
{
  success: true,
  suggestions: {
    title: string;
    context: string;            // e.g., "14 Tage her" or "Frühling"
    suggestedAssigneeId?: string;
    suggestedXP?: number;
    suggestedIcon?: string;
  }[]                           // max 3 items
}
```

**Auth:** Adults only.
**Implementation:** Server calls Anthropic API with anonymized task
history. Response cached for 1 hour per family.

---

## 7. Child Avatar (C-6)

### GET `/api/avatars/:userId`

**Response (200):**
```typescript
{
  success: true,
  avatar: {
    userId: string;
    skinTone: string;
    hairStyle: string;
    top: string;
    bottom: string;
    accessory: string | null;
    background: string;
    updatedAt: string;
  } | null                      // null if no avatar created
}
```

**Auth:** Required. Own avatar or family member.

### PUT `/api/avatars/:userId`

**Request body:**
```typescript
{
  skinTone: string;
  hairStyle: string;
  top: string;
  bottom: string;
  accessory?: string;
  background: string;
}
```

**Response (200):** `{ success: true, avatar: ChildAvatar }`
**Auth:** Required. Children can only edit own avatar.

### GET `/api/avatars/items`

Returns all avatar items with unlock requirements.

**Response (200):**
```typescript
{
  success: true,
  items: {
    id: string;
    category: "skinTone" | "hairStyle" | "top" | "bottom" | "accessory" | "background";
    name: string;
    assetKey: string;           // references the SVG/image asset
    unlockType: "level" | "badge" | "default";
    unlockRequirement?: number | string; // level number or badge ID
  }[]
}
```

**Auth:** Required.

---

## 8. Multi-Household (S-2)

### POST `/api/families/:familyId/link-child`

**Request body:**
```typescript
{
  childUsername: string;
}
```

**Response (201):**
```typescript
{
  success: true,
  linkRequest: {
    id: string;
    childUserId: string;
    targetFamilyId: string;
    status: "pending";
  }
}
```

**Auth:** Adults only.
**Error:** 404 if username not found (generic error, no username leak).
**Error:** 409 if child already in 2 families.

### POST `/api/family-links/:linkId/confirm`

**Request body:**
```typescript
{
  pin: string;                  // child's PIN for verification
}
```

**Response (200):** `{ success: true }`
**Auth:** Required. Child whose username was used.

### POST `/api/family-links/:linkId/decline`

**Response (200):** `{ success: true }`
**Auth:** Required. Child whose username was used.

### GET `/api/families/mine`

Returns all families the current user belongs to (1 or 2 for children).

**Response (200):**
```typescript
{
  success: true,
  families: {
    id: string;
    name: string;
    role: "admin" | "adult" | "child";
    memberCount: number;
  }[]
}
```

### DELETE `/api/families/:familyId/members/:userId`

Unlinks a child from a family (if child has 2 families).

**Auth:** Adults only in the target family.
**Error:** 400 if this is the child's only family.

---

## New Error Codes

| Code | HTTP | Description |
|------|------|-------------|
| `PHOTO_REQUIRED` | 400 | Task requires photo proof but none attached |
| `MAX_FAMILIES_REACHED` | 409 | Child already belongs to 2 families |
| `LINK_PENDING` | 409 | A link request already exists for this child+family |
| `LINK_NOT_FOUND` | 404 | Link request not found |
| `NUDGE_LIMIT_EXCEEDED` | 400 | More than 3 nudge times configured |

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.1.0 | 2026-03-30 | Added 8 new feature endpoints | PM + VEGA |

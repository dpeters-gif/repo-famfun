---
document: api-contracts
project: "Familienzentrale"
version: 1.0.0
status: Draft
last-updated: "2026-03-29"
depends-on:
  - constitution.md
  - shared/schema.ts
---

# API Contracts

> This document defines NEW and CHANGED endpoints for v2.
> Existing endpoints that remain unchanged are listed in Section 1
> for reference only. The builder must follow these contracts exactly.
>
> All endpoints return errors in the standard shape:
> `{ error: { code: string, message: string } }`
>
> All endpoints requiring auth check `req.session.userId`.
> All endpoints accessing family data verify user belongs to that family.

---

## 1. Existing Endpoints (Unchanged — reference only)

These endpoints exist and remain as-is. Do not modify without PM approval.

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/api/health` | Health check |
| GET | `/api/db-health` | Database connectivity |
| POST | `/api/auth/signup` | Create adult account |
| POST | `/api/auth/login` | Login (email or username) |
| POST | `/api/auth/logout` | Logout |
| GET | `/api/auth/me` | Current user profile |
| PATCH | `/api/users/me` | Update own profile |
| POST | `/api/families` | Create family |
| GET | `/api/families/me` | Get current family + members |
| POST | `/api/families/:familyId/invite-adult` | Create invite token |
| POST | `/api/families/accept-invite` | Accept invite |
| POST | `/api/families/:familyId/children` | Add child |
| GET | `/api/families/:familyId/children/:childUserId/permissions` | Get child permissions |
| PATCH | `/api/families/:familyId/children/:childUserId/permissions` | Update child permissions |
| POST | `/api/family-tags` | Create tag |
| GET | `/api/family-tags` | List tags |
| GET | `/api/notifications` | List notifications |
| POST | `/api/notifications/:id/read` | Mark notification read |

---

## 2. Changed Endpoints

### POST `/api/tasks` (CHANGED)

Added fields for v2 gamification.

**Request body:**
```typescript
{
  title: string;              // required
  description?: string;
  familyId: string;           // required
  assignedToUserId?: string;
  visibility: "private_to_creator" | "shared_in_family";
  priority: "low" | "normal" | "high";  // default: "normal"
  dueDate?: string;           // YYYY-MM-DD
  startTime?: string;         // HH:mm
  endTime?: string;           // HH:mm
  xpValue: number;            // 0-100, replaces pointsValue
  icon?: string;              // icon identifier, default per type
  challengeId?: string;       // link to active challenge (optional)
}
```

**Changes from v1:** `pointsValue` renamed to `xpValue`. Added `icon`, `challengeId`.

**Response (201):**
```typescript
{ success: true, task: Task }
```

**Auth:** Required. Both adults and children (if `canCreateTasks` permission enabled).
**Role check:** If child, verify `canCreateTasks` permission. Children can only assign to self.

---

### POST `/api/tasks/:taskId/complete` (CHANGED)

Now returns gamification state for animation rendering.

**Request body:** (none)

**Response (200):**
```typescript
{
  success: true,
  task: Task,
  gamification: {
    xpAwarded: number;        // XP earned from this task
    totalXP: number;          // user's total XP after award
    currentLevel: number;     // current level after award
    currentLevelXP: number;   // XP within current level
    nextLevelXP: number;      // XP needed for next level
    leveledUp: boolean;       // true if this completion triggered level-up
    newLevel?: number;        // new level number (if leveledUp)
    streakCount: number;      // current streak after completion
    streakStartedToday: boolean; // true if this was first completion today
    challengeProgress?: {     // if task contributes to a challenge
      challengeId: string;
      currentCount: number;
      targetCount: number;
      completed: boolean;
    };
    badgesEarned?: {          // badges earned by this completion
      id: string;
      name: string;
      description: string;
      icon: string;
    }[];
  }
}
```

**Auth:** Required. Task must be assigned to the requesting user (or unassigned for adults).
**Transaction:** Task completion + XP ledger write wrapped in DB transaction.

---

### POST `/api/events` (CHANGED)

Added icon and status for child-suggested events.

**Request body:**
```typescript
{
  title: string;              // required
  description?: string;
  familyId: string;           // required
  startAt: string;            // ISO 8601 timestamp
  endAt: string;              // ISO 8601 timestamp
  isAllDay: boolean;          // default: false
  assignedToUserIds?: string[]; // NEW: multi-person assignment
  icon?: string;              // icon identifier
  status?: "active" | "pending"; // default: "active". Children create as "pending"
}
```

**Changes from v1:** Added `assignedToUserIds[]`, `icon`, `status`.

**Auth:** Required. Children require `canCreateEvents` permission and events are created as `status: "pending"`.

---

### PATCH `/api/families/:familyId/children/:childUserId/permissions` (CHANGED)

Added `canCreateEvents`.

**Request body:**
```typescript
{
  canCreateTasks?: boolean;
  canCreateEvents?: boolean;   // NEW
}
```

---

## 3. New Endpoints

### Time Blocks

#### POST `/api/time-blocks`

**Request body:**
```typescript
{
  familyId: string;
  userId: string;             // which member this block is for
  type: "school" | "work" | "unavailable";
  title: string;
  weekdays: number[];         // 0=Mon, 1=Tue, ..., 6=Sun
  startTime: string;          // HH:mm
  endTime: string;            // HH:mm
}
```

**Response (201):**
```typescript
{ success: true, timeBlock: TimeBlock }
```

**Auth:** Required. Adults only.

#### GET `/api/time-blocks`

**Query params:** `familyId` (required)

**Response (200):**
```typescript
{ success: true, timeBlocks: TimeBlock[] }
```

**Auth:** Required. Returns all blocks for the family (all members see all blocks).

#### PATCH `/api/time-blocks/:id`

**Request body:** Partial TimeBlock fields.
**Response (200):** `{ success: true, timeBlock: TimeBlock }`
**Auth:** Adults only.

#### DELETE `/api/time-blocks/:id`

**Response (200):** `{ success: true }`
**Auth:** Adults only.

---

### Gamification

#### GET `/api/gamification/profile/:userId`

Returns complete gamification state for a user.

**Response (200):**
```typescript
{
  success: true,
  profile: {
    totalXP: number;
    currentLevel: number;
    currentLevelXP: number;
    nextLevelXP: number;
    streak: {
      current: number;
      longest: number;
      lastCompletedDate: string | null;
      isAtRisk: boolean;      // true if >18:00 and no completion today
    };
    badges: {
      id: string;
      name: string;
      description: string;
      icon: string;
      earnedAt: string;
    }[];
    availableBadges: {        // locked badges with criteria hints
      id: string;
      name: string;
      icon: string;
      category: string;
      hint: string;           // e.g., "Complete a 7-day streak"
    }[];
  }
}
```

**Auth:** Required. Users can view own profile. Adults can view any family member's profile.

#### GET `/api/gamification/leaderboard`

**Query params:** `familyId` (required), `period: "weekly" | "monthly"`

**Response (200):**
```typescript
{
  success: true,
  period: "weekly" | "monthly",
  rankings: {
    position: number;
    userId: string;
    name: string;
    xp: number;
    positionChange: number;   // +2, -1, 0 compared to previous snapshot
  }[]
}
```

**Auth:** Required. Returns children only.

#### GET `/api/gamification/streak-history/:userId`

**Query params:** `days: number` (default: 30)

**Response (200):**
```typescript
{
  success: true,
  history: {
    date: string;             // YYYY-MM-DD
    completed: boolean;       // had at least one completion
    xpEarned: number;         // total XP earned that day
  }[]
}
```

---

### Challenges

#### POST `/api/challenges`

**Request body:**
```typescript
{
  familyId: string;
  title: string;
  description?: string;
  type: "individual" | "family";
  targetCount: number;
  targetMetric: "tasks_completed";
  startDate: string;          // YYYY-MM-DD
  endDate: string;            // YYYY-MM-DD
  rewardXP?: number;          // bonus XP on completion
}
```

**Response (201):** `{ success: true, challenge: Challenge }`
**Auth:** Adults only.

#### GET `/api/challenges`

**Query params:** `familyId` (required), `status?: "active" | "completed" | "expired"`

**Response (200):**
```typescript
{
  success: true,
  challenges: {
    ...Challenge,
    progress: {
      currentCount: number;
      targetCount: number;
      contributors: {
        userId: string;
        name: string;
        count: number;
      }[];
    }
  }[]
}
```

**Auth:** Required. All family members can view.

#### GET `/api/challenges/:id`

**Response (200):** Single challenge with progress (same shape as list item).

---

### Calendar Sync

#### POST `/api/calendar-connections`

Initiates OAuth flow.

**Request body:**
```typescript
{
  provider: "google" | "apple";
  redirectUri: string;
}
```

**Response (200):**
```typescript
{ success: true, authUrl: string }
```

#### POST `/api/calendar-connections/callback`

Handles OAuth callback.

**Request body:**
```typescript
{
  provider: "google" | "apple";
  code: string;
}
```

**Response (201):**
```typescript
{
  success: true,
  connection: CalendarConnection,
  calendars: { id: string; name: string; color: string }[]
}
```

#### PATCH `/api/calendar-connections/:id`

Configure which calendars to sync.

**Request body:**
```typescript
{
  selectedCalendars: string[];  // external calendar IDs
  syncDirection: "import" | "export" | "both";
  syncEnabled: boolean;
}
```

#### DELETE `/api/calendar-connections/:id`

Disconnects calendar. Deletes all associated external events and tokens.

**Response (200):** `{ success: true }`

#### GET `/api/external-events`

**Query params:** `familyId`, `start` (ISO), `end` (ISO)

Returns external calendar events for the date range.

**Response (200):**
```typescript
{
  success: true,
  events: ExternalCalendarEvent[]
}
```

---

### Event Approval (child-suggested events)

#### POST `/api/events/:eventId/approve`

Parent approves a child-suggested event (status: pending → active).

**Request body:** (none)
**Response (200):** `{ success: true, event: Event }`
**Auth:** Adults only.

#### POST `/api/events/:eventId/reject`

Parent rejects a child-suggested event.

**Request body:** `{ reason?: string }`
**Response (200):** `{ success: true }`
**Auth:** Adults only.

#### GET `/api/events/pending`

Lists all pending (child-suggested) events for the family.

**Response (200):**
```typescript
{
  success: true,
  events: Event[]  // where status = "pending"
}
```

**Auth:** Adults only.

---

### Care-Share

#### GET `/api/care-share`

**Query params:** `familyId` (required), `period: "weekly" | "monthly"`

**Response (200):**
```typescript
{
  success: true,
  period: "weekly" | "monthly",
  distribution: {
    userId: string;
    name: string;
    completedCount: number;
    percentage: number;
  }[]
}
```

**Auth:** Adults only. Returns 403 for child sessions.

---

### Calendar View (CHANGED)

#### GET `/api/calendar/view/family` (CHANGED)

Now includes time blocks and external events.

**Query params:** `familyId`, `start` (ISO), `end` (ISO)

**Response (200):**
```typescript
{
  success: true,
  timeBlocks: TimeBlock[],
  events: Event[],
  externalEvents: ExternalCalendarEvent[],
  tasks: Task[],              // tasks with dueDate in range
  routineTasks: Task[]        // routine-generated tasks in range (with meta.routineId)
}
```

---

## 4. Error Codes (New)

| Code | HTTP | Description |
|------|------|-------------|
| `CHILD_NOT_PERMITTED` | 403 | Child lacks required permission |
| `EVENT_PENDING` | 400 | Cannot modify pending event (approve/reject first) |
| `CHALLENGE_EXPIRED` | 400 | Challenge has ended |
| `CALENDAR_AUTH_FAILED` | 400 | OAuth flow failed |
| `CALENDAR_SYNC_FAILED` | 500 | Sync fetch failed |
| `LEVEL_CALCULATION_ERROR` | 500 | XP/level inconsistency detected |

---

## Changelog

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0.0 | 2026-03-29 | Initial API contracts for v2 | PM + VEGA |

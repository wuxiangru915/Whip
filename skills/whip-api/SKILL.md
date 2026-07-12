---
name: whip-api
description: "Design stable, well-documented APIs and interfaces. Use when designing REST endpoints, module boundaries, component props, or any public interface between systems. Covers contract-first design, error semantics, validation boundaries, and Hyrum's Law."
---

# API Design

## Overview

Design stable interfaces that are hard to misuse. Good interfaces make the right thing easy and the wrong thing hard.

## When to Use

- Designing new API endpoints
- Defining module boundaries or contracts between teams
- Creating component prop interfaces
- Changing existing public interfaces

## Core Principles

### Hyrum's Law

> With enough users, ALL observable behaviors will be depended on by somebody.

Every public behavior — including undocumented quirks, error message text, timing, ordering — becomes a de facto contract. Design implications:
- Be intentional about what you expose
- Don't leak implementation details
- Plan for deprecation at design time

### Contract First

Define the interface before implementing it. The contract is the spec.

```typescript
interface TaskAPI {
  createTask(input: CreateTaskInput): Promise<Task>;
  listTasks(params: ListTasksParams): Promise<PaginatedResult<Task>>;
  getTask(id: string): Promise<Task>;
  updateTask(id: string, input: UpdateTaskInput): Promise<Task>;
  deleteTask(id: string): Promise<void>;
}
```

### Consistent Error Semantics

Pick one error format and use it everywhere:

```typescript
interface APIError {
  error: {
    code: string;        // Machine-readable: "VALIDATION_ERROR"
    message: string;     // Human-readable: "Email is required"
    details?: unknown;   // Additional context
  };
}

// Status code mapping:
// 400 → invalid data
// 401 → not authenticated
// 403 → not authorized
// 404 → not found
// 409 → conflict (duplicate, version mismatch)
// 422 → validation failed
// 500 → server error (never expose internals)
```

Don't mix patterns. If some endpoints throw, others return null, others return `{ error }` — the consumer can't predict behavior.

### Validate at Boundaries

```typescript
app.post('/api/tasks', async (req, res) => {
  const result = CreateTaskSchema.safeParse(req.body);
  if (!result.success) {
    return res.status(422).json({
      error: { code: 'VALIDATION_ERROR', details: result.error.flatten() }
    });
  }
  // After validation, internal code trusts the types
  const task = await taskService.create(result.data);
  return res.status(201).json(task);
});
```

Where validation belongs: API routes, form handlers, **third-party API responses** (always untrusted).
Where it does NOT belong: between internal functions, on data from your own DB.

### Prefer Addition Over Modification

```typescript
// Good: add optional fields (backward compatible)
interface CreateTaskInput {
  title: string;
  description?: string;
  priority?: 'low' | 'medium' | 'high';  // added later
}

// Bad: change existing fields (breaks consumers)
interface CreateTaskInput {
  title: string;
  priority: number;  // changed from string — breaks existing code
}
```

### Naming Conventions

| Pattern | Convention | Example |
|---------|-----------|---------|
| REST endpoints | Plural nouns, no verbs | `GET /api/tasks` |
| Query params | camelCase | `?sortBy=createdAt&pageSize=20` |
| Response fields | camelCase | `{ createdAt, updatedAt }` |
| Boolean fields | is/has/can prefix | `isComplete`, `hasAttachments` |
| Enum values | UPPER_SNAKE | `"IN_PROGRESS"` |

## REST Resource Design

```
GET    /api/tasks              → List (with query filters)
POST   /api/tasks              → Create
GET    /api/tasks/:id          → Get single
PATCH  /api/tasks/:id          → Partial update
DELETE /api/tasks/:id          → Delete

GET    /api/tasks/:id/comments → Sub-resource list
POST   /api/tasks/:id/comments → Sub-resource create
```

**Pagination** on all list endpoints:
```json
{
  "data": [...],
  "pagination": { "page": 1, "pageSize": 20, "totalItems": 142, "totalPages": 8 }
}
```

## TypeScript Patterns

### Discriminated Unions for Variants
```typescript
type TaskStatus =
  | { type: 'pending' }
  | { type: 'in_progress'; assignee: string }
  | { type: 'completed'; completedAt: Date }
  | { type: 'cancelled'; reason: string };
```

### Input/Output Separation
```typescript
interface CreateTaskInput { title: string; description?: string; }
interface Task { id: string; title: string; createdAt: Date; createdBy: string; }
```

### Branded Types for IDs
```typescript
type TaskId = string & { readonly __brand: 'TaskId' };
type UserId = string & { readonly __brand: 'UserId' };
// Prevents passing UserId where TaskId expected
```

## Red Flags

- Endpoints returning different shapes depending on conditions
- Inconsistent error formats across endpoints
- Validation scattered throughout internal code
- Breaking changes to existing fields
- List endpoints without pagination
- Verbs in REST URLs (`/api/createTask`)
- Third-party API responses used without validation

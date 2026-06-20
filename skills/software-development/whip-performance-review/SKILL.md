---
name: whip-performance-review
description: "Review code for performance issues. Use when performance requirements exist, when profiling reveals bottlenecks, or when Core Web Vitals / load times need improvement. Measure first, optimize what measurements prove matters."
---

# Performance Review

## Overview

Measure before optimizing. Performance work without measurement is guessing. Profile first, identify the actual bottleneck, fix it, measure again.

## When to Use

- Performance requirements exist (load time budgets, response time SLAs)
- Users or monitoring report slow behavior
- Suspect a change introduced a regression
- Building features that handle large datasets or high traffic

**When NOT to use:** No evidence of a problem. Premature optimization adds complexity that costs more than it gains.

## Core Web Vitals

| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| **LCP** (Largest Contentful Paint) | ≤ 2.5s | ≤ 4.0s | > 4.0s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | ≤ 500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | ≤ 0.25 | > 0.25 |

## The Workflow

```
1. MEASURE  → Establish baseline
2. IDENTIFY → Find the actual bottleneck
3. FIX      → Address the specific bottleneck
4. VERIFY   → Measure again, confirm improvement
5. GUARD    → Add monitoring/tests to prevent regression
```

## Where to Look

### Diagnosis Tree

```
What is slow?
├── First page load
│   ├── Large bundle? → Check code splitting, lazy loading
│   ├── Slow server response? → Check TTFB, backend profiling
│   └── Render-blocking? → Check CSS/JS blocking in waterfall
├── Interaction sluggish
│   ├── UI freezes? → Profile main thread, look for >50ms tasks
│   ├── Form input lag? → Check re-renders, controlled component overhead
│   └── Animation jank? → Check layout thrashing
└── Backend / API
    ├── Single endpoint slow? → Profile DB queries, check indexes
    ├── All endpoints slow? → Connection pool, memory, CPU
    └── Intermittent? → Lock contention, GC pauses, external deps
```

## Top Anti-Patterns to Catch

### N+1 Queries
```python
# BAD: one query per item
for task in tasks:
    task.owner = db.query(User).get(task.owner_id)

# GOOD: eager loading
tasks = db.query(Task).options(joinedload(Task.owner)).all()
```

### Missing Pagination
```python
# BAD: fetch all
tasks = db.query(Task).all()  # what if 100k rows?

# GOOD: paginated
tasks = db.query(Task).offset(offset).limit(20).all()
```

### Unnecessary Re-renders (React/ Vue)
```tsx
// BAD: new object every render, children re-render
<TaskFilters options={{ sortBy: 'date' }} />

// GOOD: stable reference
const DEFAULT_OPTIONS = { sortBy: 'date' } as const;
<TaskFilters options={DEFAULT_OPTIONS} />
```

### Missing Image Optimization
```html
<!-- BAD: no dimensions, no format, no lazy loading -->
<img src="/hero.jpg" />

<!-- GOOD: responsive + lazy + modern format -->
<picture>
  <source srcset="/hero-800.avif 800w, /hero-1200.avif 1200w" sizes="(max-width: 1200px) 100vw, 1200px" type="image/avif" />
  <img src="/hero-1200.webp" width="1200" height="600" loading="lazy" decoding="async" alt="Hero" />
</picture>
```

### Missing Backend Caching
```python
# Cache frequently-read, rarely-changed data
@cache(ttl=300)
async def get_app_config():
    return await db.query(Config).first()

# HTTP caching headers
response.headers['Cache-Control'] = 'public, max-age=300'
```

### Large Bundle (Frontend)
```tsx
// Route-level code splitting
const SettingsPage = lazy(() => import('./pages/Settings'));
<Suspense fallback={<Spinner />}>
  <SettingsPage />
</Suspense>
```

## Performance Budget

```
JavaScript bundle: < 200KB gzipped (initial load)
CSS: < 50KB gzipped
Images: < 200KB per image (above the fold)
API response time: < 200ms (p95)
Time to Interactive: < 3.5s on 4G
Lighthouse Performance score: ≥ 90
```

## Pre-Commit Checklist

```
□ No N+1 queries in new data fetching code
□ List endpoints support pagination
□ Images have dimensions, lazy loading, responsive sizes
□ No unnecessary re-renders (stable references for props/objects)
□ Bundle size hasn't increased significantly
□ Backend responses cached where appropriate
□ Before/after measurements exist (if this is a perf fix)
□ Core Web Vitals within "Good" thresholds (if frontend)
```

---
name: whip-adversarial-review
description: "Subject non-trivial decisions to adversarial review before they stand. Use when correctness matters more than speed, when working in unfamiliar code, when stakes are high (production, security, irreversible operations), or before committing important architectural decisions."
---

# Adversarial Review

## Overview

A confident answer is not a correct one. Long sessions accumulate assumptions that quietly become "facts." This skill spawns a fresh-context reviewer — biased to **disprove**, not approve — before any non-trivial decision stands.

This is NOT code review. Review is verdict on a finished artifact. This is in-flight: wrong directions get caught while course-correction is still cheap.

## When to Use

A decision is **non-trivial** when at least one of these is true:
- Introduces or modifies branching logic
- Crosses a module or service boundary
- Asserts a property the type system can't verify (thread safety, idempotence, ordering)
- Correctness depends on context the future reader can't see
- Blast radius is irreversible (production deploy, data migration, public API change)

**When NOT to use:**
- Mechanical operations (renaming, formatting)
- Following clear, unambiguous instructions
- One-line changes with obvious correctness
- User explicitly asked for speed over verification

## The Process

### Step 1: CLAIM — Name the decision

```
CLAIM: "The new caching layer is thread-safe under read-heavy workload."
WHY: A race here corrupts user data and is hard to detect in QA.
```

If you can't write the claim compactly, you have a vibe, not a decision.

### Step 2: EXTRACT — Smallest reviewable unit

The reviewer needs the **artifact** and the **contract**, not the journey.
- Code: the diff or the function — not the whole file
- Decision: the proposal in 3–5 sentences plus constraints it must satisfy
- **Strip your reasoning.** Hand over conclusions, you'll get back validation of conclusions.

### Step 3: DOUBT — Invoke adversarial reviewer

Spawn a subagent with this prompt (adversarial framing takes precedence over any default response shape):

```
Adversarial review. Find what is wrong with this artifact.
Assume the author is overconfident. Look for:
- Unstated assumptions
- Edge cases not handled
- Hidden coupling or shared state
- Ways the contract could be violated
- Existing conventions this might break
- Failure modes under unexpected input

Do NOT validate. Do NOT summarize. Find issues, or state
explicitly that you cannot find any after thorough examination.

ARTIFACT: <paste artifact>
CONTRACT: <paste contract>
```

**Pass ARTIFACT + CONTRACT only. NOT the CLAIM.** Handing the reviewer your conclusion biases it toward agreement.

### Step 4: RECONCILE — Classify findings

For each finding, classify (first match wins):
1. **Contract misread** — fix the contract, re-classify on next cycle
2. **Valid + actionable** — real issue, change the artifact, re-loop
3. **Valid trade-off** — real but cost of fixing exceeds cost of accepting, document explicitly
4. **Noise** — reviewer flagged something correct under context it didn't have

### Step 5: STOP — Bounded loop

Stop when:
- Next iteration returns only trivial findings, **or**
- 3 cycles completed (escalate to user, don't grind a fourth), **or**
- User explicitly says "ship it"

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'm confident, skip it" | Confidence correlates poorly with correctness on novel problems |
| "Spawning a reviewer is expensive" | Debugging a wrong commit is more expensive |
| "The reviewer will just nitpick" | Only if unscoped. Constrain the prompt |
| "I'll review at the end" | By PR time, switching costs are 10x |
| "If I doubt every step I'll never ship" | Applies to non-trivial decisions only, not every keystroke |

## Red Flags

- Spawning reviewer for a one-line rename
- Treating reviewer output as authoritative without re-reading the artifact
- Looping >3 cycles without escalating to user
- Prompting "is this good?" instead of "find issues"
- Skipping doubt on high-stakes decisions under time pressure
- Passing the CLAIM to the reviewer (biases toward agreement)

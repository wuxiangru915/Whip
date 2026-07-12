---
name: whip-interview
description: "Extract what the user actually wants through structured questioning. Use when the ask is underspecified ('build me X' without who/why), when the user says 'interview me' or 'are we sure?', or when you catch yourself filling in ambiguous requirements before any plan exists."
---

# Requirements Interview

## Overview

What people ask for and what they actually want are different things. The cheapest moment to find this gap is before any plan, spec, or code exists.

## When to Use

- The ask is missing at least one of: **who**, **why**, what **success** looks like, what the binding **constraint** is
- The request is conventional rather than specific ("build me a dashboard", "make it faster")
- You're tempted to start with assumptions you haven't surfaced
- User explicitly invokes: "interview me", "are we sure?", "stress-test my thinking"

**When NOT to use:**
- Unambiguous ask ("rename this variable", "fix this typo")
- User explicitly asked for speed over verification
- Pure information requests ("how does X work?")

## The Process

### Step 1: Hypothesize with confidence

Write your current best read in **one sentence**, plus a confidence number (0–100%):

```
HYPOTHESIS: You want a way to track experiments, and "dashboard" was the convention that came to mind.
CONFIDENCE: ~30% — missing: who it's for, what "metrics" means, what success looks like
```

Below ~70%, append a brief reason — what's still unresolved.

### Step 2: Ask one question at a time, each with a guess

```
Q: When you say "how are we doing?", who's asking — you alone or the team in standup?
GUESS: engineering team in standup, because "we" usually scopes that way.
```

**Why one at a time:** The third question often depends on the first answer. Batches encourage skim-reading.

**Why attach a guess:** Reacting to a wrong guess is faster than generating from scratch. It surfaces your assumptions.

### Step 3: Listen for "want vs. should want"

Watch for answers that pattern-match best-practice talk ("scalable", "clean architecture") without specifics. When you hear these:

> *"If you didn't have to justify this to anyone, what would you actually want?"*

### Step 4: Restate intent

When confidence is high, write back what you now think the user wants (5–8 lines):

```
Here's what I now think you want:

- Outcome:      <one line>
- User:         <one line>
- Why now:      <one line>
- Success:      <one line>
- Constraint:   <one line>
- Out of scope: <one line>

Yes / no / refine?
```

**"Out of scope" is non-negotiable.** Half of misalignment is silent disagreement about what is NOT being built.

### Step 5: Confirm — explicit yes

Not "sounds good", not "whatever you think". If they correct you, fold the correction in and restate. Loop until explicit yes.

## The 95% Confidence Stop

> *Can I predict the user's reaction to the next three questions I would ask?*

If yes, stop interviewing. If no, ask the next question.

## Idea Refinement (Divergent → Convergent)

When the confirmed intent is "I want X but I don't know how to scope it":

**Phase 1 — Expand (Divergent):**
1. Restate as a "How Might We" problem statement
2. Ask 3-5 sharpening questions (who, success, constraints, why now)
3. Generate 5-8 idea variations using lenses: Inversion, Constraint removal, Audience shift, Simplification, 10x version

**Phase 2 — Converge:**
1. Cluster into 2-3 distinct directions
2. Stress-test each: user value, feasibility, differentiation
3. Surface hidden assumptions (what could kill this, what you're betting is true)

**Phase 3 — Ship:**
Produce a concrete one-pager:
- Problem Statement
- Recommended Direction
- Key Assumptions to Validate
- MVP Scope
- **Not Doing** list (the most valuable part — focus is about saying no)

## Red Flags

- Three or more questions in a single message (batching, not interviewing)
- A question without your hypothesis attached
- Accepting "whatever you think" as a terminal answer
- Producing a spec/plan before user confirmed your restate
- Generating 20+ shallow ideas instead of 5-8 considered ones
- Skipping "who is this for"

## Downstream

After confirmed intent → `whip-brain` for design → `whip-plan` for implementation plan.

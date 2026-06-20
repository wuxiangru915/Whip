<div align="center">

# 🐎 Whip

**Production-grade engineering skills for AI coding agents.**

A curated fusion of [obra/superpowers](https://github.com/obra/superpowers) and [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — 19 skills covering the full software development lifecycle.

![Skills](https://img.shields.io/badge/skills-19-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)

</div>

---

## Why Whip? 

Recently, the AI engineering community has been obsessed with "harnessing" agents and workflows. Everyone is trying to put a **harness** on AI to control it.

But we thought: *Why just harness it when you can drive it forward?*

While others are busy fitting the leather straps and buckles to build a **harness**, **Whip** gives you the whip to actively drive and orchestrate your `Superpowers` and `Agent-skills`.

Don't just restrain AI engineering—**whip it into action**.

---

## Installation

### Option 1: Hermes Agent Plugin (Recommended)

```bash
# Install via agy CLI
agy plugin install https://github.com/wuxiangru915/Whip

# Verify installation
agy skills list | grep whip
```

### Option 2: Manual Install

```bash
# Clone the repo
git clone https://github.com/wuxiangru915/Whip.git

# Copy skills into your Hermes skills directory
cp -r Whip/skills/* ~/.hermes/skills/

# Or into any compatible agent's skill directory
cp -r Whip/skills/* /path/to/your/agent/skills/
```

### Option 3: Cherry-Pick Individual Skills

Only need specific skills? Copy just what you need:

```bash
git clone https://github.com/wuxiangru915/Whip.git

# Pick individual skills
cp -r Whip/skills/superpowers/whip-brainstorming ~/.hermes/skills/
cp -r Whip/skills/software-development/whip-security-review ~/.hermes/skills/
```

---

## All Skills & Invoke Commands

### 🧠 Define — Clarify what to build

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-brainstorming` | Before any creative work — creating features, building components, adding functionality. Explores user intent, requirements and design before implementation. |
| `whip-requirements-interview` | When the ask is underspecified ("build me X" without who/why), when user says "interview me" or "are we sure?", or when you catch yourself filling in ambiguous requirements before any plan exists. |

### 📋 Plan — Break it down

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-writing-plans` | When you have a spec or requirements for a multi-step task, before touching code. Produces bite-sized tasks with file paths and acceptance criteria. |

### 🔨 Build — Write the code

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-test-driven-development` | When implementing any feature or bugfix, before writing implementation code. Enforces RED-GREEN-REFACTOR. |
| `whip-subagent-driven-development` | When executing implementation plans with independent tasks. Spawns subagents for parallel execution with 2-stage review. |
| `whip-executing-plans` | When you have a written implementation plan to execute in a separate session with review checkpoints. |
| `whip-dispatching-parallel-agents` | When facing 2+ independent tasks that can be worked on without shared state or sequential dependencies. |
| `whip-using-git-worktrees` | When starting feature work that needs isolation from current workspace. Creates isolated workspace via git worktree. |

### 🔍 Verify — Prove it works

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-verification-before-completion` | When about to claim work is complete, fixed, or passing. Requires running verification commands and confirming output before making any success claims. |
| `whip-systematic-debugging` | When encountering any bug, test failure, or unexpected behavior. 4-phase root cause debugging: understand before fixing. |

### 👀 Review — Quality gates before merge

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-adversarial-review` | When correctness matters more than speed. Fresh-context adversarial review for non-trivial decisions — biased to disprove, not approve. |
| `whip-security-review` | Before committing code that handles user input, authentication, data storage, or external integrations. OWASP Top 10, SSRF, LLM security. |
| `whip-performance-review` | When performance requirements exist, when profiling reveals bottlenecks, or when Core Web Vitals / load times need improvement. |
| `whip-api-design` | When designing REST endpoints, module boundaries, component props, or any public interface. Contract-first design, Hyrum's Law, error semantics. |
| `whip-requesting-code-review` | When completing tasks, implementing major features, or before merging. Pre-commit review: security scan, quality gates, auto-fix. |
| `whip-receiving-code-review` | When receiving code review feedback, before implementing suggestions. Requires technical rigor and verification, not performative agreement. |

### 🚀 Ship — Deploy with confidence

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-finishing-a-development-branch` | When implementation is complete, all tests pass, and you need to decide how to integrate the work. Merge, PR, or cleanup. |

### 🛠 Meta — Skills about skills

| Invoke Command | When to Use |
|----------------|-------------|
| `whip-using-superpowers` | When starting any conversation. Establishes how to find and use skills, requiring Skill tool invocation before ANY response. |
| `whip-writing-skills` | When creating new skills, editing existing skills, or verifying skills work before deployment. |

---

## Pipeline Flow

```
User request
    │
    ▼
┌──────────────────────────────┐
│  whip-requirements-interview │  Ask is underspecified?
│                              │  → Extract real intent
└────────────┬─────────────────┘
             ▼
┌──────────────────────────────┐
│  whip-brainstorming          │  Design exploration
│                              │  + interview-me techniques
└────────────┬─────────────────┘
             ▼
┌──────────────────────────────┐
│  whip-writing-plans          │  Implementation plan
└────────────┬─────────────────┘
             ▼
┌──────────────────────────────┐
│  whip-subagent-driven-       │  Auto-execute tasks
│  development                 │  with TDD
└────────────┬─────────────────┘
             ▼
┌──────────────────────────────┐
│  whip-verification-before-   │  Evidence before claims
│  completion                  │
├──────────────────────────────┤
│  whip-security-review        │  Pre-commit gates
│  whip-performance-review     │
│  whip-adversarial-review     │  (high-risk decisions)
└────────────┬─────────────────┘
             ▼
┌──────────────────────────────┐
│  whip-requesting-code-review │  Code review
└────────────┬─────────────────┘
             ▼
┌──────────────────────────────┐
│  whip-finishing-a-           │  Ship it
│  development-branch          │
└──────────────────────────────┘
```

---

## Quick Reference

```bash
# Start a new feature
"Let's build a user dashboard"
# → whip-brainstorming → whip-writing-plans → whip-subagent-driven-development

# Fix a bug
"The login form throws 500 on empty email"
# → whip-systematic-debugging → whip-test-driven-development

# Review before commit
"Review this code before I push"
# → whip-security-review + whip-performance-review + whip-requesting-code-review

# Design an API
"Design the REST API for our task manager"
# → whip-api-design → whip-writing-plans

# Validate an idea
"I want to add real-time notifications"
# → whip-requirements-interview → whip-brainstorming
```

---

## What's NOT Included (and Why)

| agent-skills skill | Why not |
|---|---|
| context-engineering | Theoretical, limited practical value |
| source-driven-development | Over-idealized workflow |
| browser-testing-with-devtools | Requires MCP Chrome setup |
| deprecation-and-migration | Too niche |
| observability-and-instrumentation | Overkill for small-to-mid projects |
| ci-cd-and-automation | Covered by existing GitHub workflow skills |
| code-simplification | Absorbed into code review process |
| documentation-and-adrs | Covered by existing capabilities |

---

## Credits

Built by fusing two excellent open-source projects:

- **[obra/superpowers](https://github.com/obra/superpowers)** — Execution engine, TDD discipline, subagent orchestration
- **[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)** — Quality gates, security patterns, API design, requirements extraction

## License

MIT

<div align="center">

# ⚡ mypower

**Production-grade engineering skills for AI coding agents.**

A curated fusion of [obra/superpowers](https://github.com/obra/superpowers) and [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — 19 skills covering the full software development lifecycle.

![Skills](https://img.shields.io/badge/skills-19-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)

</div>

---

## What is this?

`mypower` is a skill pack for [Hermes Agent](https://hermes-agent.nousresearch.com/) (and compatible AI coding agents) that encodes senior engineering workflows into reusable, triggerable skills.

It combines:
- **Superpowers' execution engine** — subagent-driven development, TDD, git worktrees, plan execution
- **Agent-skills' quality gates** — adversarial review, security hardening, performance budgets, API design, requirements extraction

The result is a complete **Define → Plan → Build → Verify → Review → Ship** pipeline.

## Skills Overview

### 🧠 Define Phase

| Skill | Purpose |
|-------|---------|
| [brainstorming](skills/superpowers/brainstorming/) | Design exploration with interview-me techniques — hypothesis + confidence, want-vs-should-want detection |
| [requirements-interview](skills/software-development/requirements-interview/) | Extract what the user actually wants through structured one-at-a-time questioning with guesses |

### 📋 Plan Phase

| Skill | Purpose |
|-------|---------|
| [writing-plans](skills/superpowers/writing-plans/) | Bite-sized implementation plans with file paths, acceptance criteria, and execution order |

### 🔨 Build Phase

| Skill | Purpose |
|-------|---------|
| [test-driven-development](skills/superpowers/test-driven-development/) | RED-GREEN-REFACTOR — no production code without a failing test first |
| [subagent-driven-development](skills/software-development/subagent-driven-development/) | Execute plans via delegate_task subagents with 2-stage review |
| [executing-plans](skills/superpowers/executing-plans/) | Run implementation plans in separate sessions with review checkpoints |
| [dispatching-parallel-agents](skills/superpowers/dispatching-parallel-agents/) | Parallel execution of independent tasks |
| [using-git-worktrees](skills/superpowers/using-git-worktrees/) | Isolated workspaces for feature work via git worktree |

### 🔍 Verify Phase

| Skill | Purpose |
|-------|---------|
| [verification-before-completion](skills/superpowers/verification-before-completion/) | Evidence before claims — run verification commands before saying "done" |
| [systematic-debugging](skills/superpowers/systematic-debugging/) | 4-phase root cause debugging: understand before fixing |

### 👀 Review Phase

| Skill | Purpose |
|-------|---------|
| [adversarial-review](skills/software-development/adversarial-review/) | Fresh-context adversarial review for non-trivial decisions — biased to disprove, not approve |
| [security-review](skills/software-development/security-review/) | OWASP Top 10, SSRF, LLM security, input validation, secrets management |
| [performance-review](skills/software-development/performance-review/) | Core Web Vitals, N+1 queries, bundle size, caching, performance budgets |
| [api-design](skills/software-development/api-design/) | Contract-first design, Hyrum's Law, REST patterns, error semantics |
| [requesting-code-review](skills/superpowers/requesting-code-review/) | Pre-commit review: security scan, quality gates, auto-fix |
| [receiving-code-review](skills/superpowers/receiving-code-review/) | Handle code review feedback with technical rigor, not performative agreement |

### 🚀 Ship Phase

| Skill | Purpose |
|-------|---------|
| [finishing-a-development-branch](skills/superpowers/finishing-a-development-branch/) | Merge, PR, or cleanup — structured options for completing work |

### 🛠 Meta

| Skill | Purpose |
|-------|---------|
| [using-superpowers](skills/superpowers/using-superpowers/) | Bootstrap: establishes how to find and use skills |
| [writing-skills](skills/superpowers/writing-skills/) | Create, edit, and verify skills before deployment |

---

## Pipeline Flow

```
User request
    │
    ▼
┌─────────────────────┐
│  requirements-       │  Ask is underspecified?
│  interview           │  → Extract real intent
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  brainstorming       │  Design exploration
│                      │  + interview-me techniques
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  writing-plans       │  Implementation plan
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  subagent-driven     │  Auto-execute tasks
│  development         │  with TDD
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  verification-       │  Evidence before claims
│  before-completion   │
├─────────────────────┤
│  security-review     │  Pre-commit gates
│  performance-review  │
│  adversarial-review  │  (high-risk decisions)
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  requesting-code-    │  Code review
│  review              │
└────────┬────────────┘
         ▼
┌─────────────────────┐
│  finishing-branch    │  Ship it
└─────────────────────┘
```

---

## Installation

### Hermes Agent

```bash
# As a plugin
agy plugin install https://github.com/wuxiangru915/mypower

# Or copy skills directly
git clone https://github.com/wuxiangru915/mypower.git
cp -r mypower/skills/* ~/.hermes/skills/
```

### Other AI Coding Agents

Each skill is a standalone `SKILL.md` file. Copy the ones you need into your agent's skill directory.

---

## What's NOT Included (and Why)

| agent-skills skill | Reason |
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

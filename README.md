<div align="center">

# ⚡ Whip

**Production-grade engineering skills for AI coding agents.**

A curated fusion of [obra/superpowers](https://github.com/obra/superpowers) and [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — 19 skills covering the full software development lifecycle.

![Skills](https://img.shields.io/badge/skills-19-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)

</div>

---

## What is Whip?

Whip is a skill pack for [Hermes Agent](https://hermes-agent.nousresearch.com/) (and compatible AI coding agents) that encodes senior engineering workflows into reusable, triggerable skills.

It combines:
- **Superpowers' execution engine** — subagent-driven development, TDD, git worktrees, plan execution
- **Agent-skills' quality gates** — adversarial review, security hardening, performance budgets, API design, requirements extraction

The result is a complete **Define → Plan → Build → Verify → Review → Ship** pipeline.

## Skills Overview

### 🧠 Define Phase

| Skill | Purpose |
|-------|---------|
| [whip-brainstorming](skills/superpowers/whip-brainstorming/) | Design exploration with interview-me techniques — hypothesis + confidence, want-vs-should-want detection |
| [whip-requirements-interview](skills/software-development/whip-requirements-interview/) | Extract what the user actually wants through structured one-at-a-time questioning with guesses |

### 📋 Plan Phase

| Skill | Purpose |
|-------|---------|
| [whip-writing-plans](skills/superpowers/whip-writing-plans/) | Bite-sized implementation plans with file paths, acceptance criteria, and execution order |

### 🔨 Build Phase

| Skill | Purpose |
|-------|---------|
| [whip-test-driven-development](skills/superpowers/whip-test-driven-development/) | RED-GREEN-REFACTOR — no production code without a failing test first |
| [whip-subagent-driven-development](skills/superpowers/whip-subagent-driven-development/) | Execute plans via delegate_task subagents with 2-stage review |
| [whip-executing-plans](skills/superpowers/whip-executing-plans/) | Run implementation plans in separate sessions with review checkpoints |
| [whip-dispatching-parallel-agents](skills/superpowers/whip-dispatching-parallel-agents/) | Parallel execution of independent tasks |
| [whip-using-git-worktrees](skills/superpowers/whip-using-git-worktrees/) | Isolated workspaces for feature work via git worktree |

### 🔍 Verify Phase

| Skill | Purpose |
|-------|---------|
| [whip-verification-before-completion](skills/superpowers/whip-verification-before-completion/) | Evidence before claims — run verification commands before saying "done" |
| [whip-systematic-debugging](skills/superpowers/whip-systematic-debugging/) | 4-phase root cause debugging: understand before fixing |

### 👀 Review Phase

| Skill | Purpose |
|-------|---------|
| [whip-adversarial-review](skills/software-development/whip-adversarial-review/) | Fresh-context adversarial review for non-trivial decisions — biased to disprove, not approve |
| [whip-security-review](skills/software-development/whip-security-review/) | OWASP Top 10, SSRF, LLM security, input validation, secrets management |
| [whip-performance-review](skills/software-development/whip-performance-review/) | Core Web Vitals, N+1 queries, bundle size, caching, performance budgets |
| [whip-api-design](skills/software-development/whip-api-design/) | Contract-first design, Hyrum's Law, REST patterns, error semantics |
| [whip-requesting-code-review](skills/superpowers/whip-requesting-code-review/) | Pre-commit review: security scan, quality gates, auto-fix |
| [whip-receiving-code-review](skills/superpowers/whip-receiving-code-review/) | Handle code review feedback with technical rigor, not performative agreement |

### 🚀 Ship Phase

| Skill | Purpose |
|-------|---------|
| [whip-finishing-a-development-branch](skills/superpowers/whip-finishing-a-development-branch/) | Merge, PR, or cleanup — structured options for completing work |

### 🛠 Meta

| Skill | Purpose |
|-------|---------|
| [whip-using-superpowers](skills/superpowers/whip-using-superpowers/) | Bootstrap: establishes how to find and use skills |
| [whip-writing-skills](skills/superpowers/whip-writing-skills/) | Create, edit, and verify skills before deployment |

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

## Installation

### Hermes Agent

```bash
# As a plugin
agy plugin install https://github.com/wuxiangru915/Whip

# Or copy skills directly
git clone https://github.com/wuxiangru915/Whip.git
cp -r Whip/skills/* ~/.hermes/skills/
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
| observability-and-instrumentation | Overkill for small-toid projects |
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

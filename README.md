<div align="center">

# Whip

**Production-grade engineering skills for AI coding agents.**

A curated fusion of [obra/superpowers](https://github.com/obra/superpowers), [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills), and [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) -- 20 skills covering the full software development lifecycle, from requirements to production-grade frontend design.

![Skills](https://img.shields.io/badge/skills-20-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)
![Platform](https://img.shields.io/badge/platform-Claude%20Code%20%7C%20Cursor%20%7C%20Gemini%20%7C%20Copilot%20%7C%20Codex-blue)

[中文文档](README-CN.md)

</div>

---

## Why Whip?

The AI engineering community is obsessed with "harnessing" agents -- putting restraints on AI to control it.

We asked: *Why just harness it when you can drive it forward?*

**Whip** gives you the whip to actively drive and orchestrate your skills. Don't just restrain AI engineering -- whip it into action.

---

## Installation

### Option 1: Manual Install (All Platforms)

```bash
git clone https://github.com/wuxiangru915/Whip.git
cp -r Whip/skills/* <your-agent-skills-dir>/
```

Skills directory by platform:

| Platform | Skills Directory |
|----------|-----------------|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| Gemini CLI | `~/.gemini/extensions/` |
| Copilot CLI | `~/.copilot/skills/` |
| Codex CLI | `~/.codex/skills/` |

### Option 2: Cherry-Pick Individual Skills

```bash
git clone https://github.com/wuxiangru915/Whip.git
cp -r Whip/skills/whip-brain <your-agent-skills-dir>/
cp -r Whip/skills/whip-security <your-agent-skills-dir>/
```

---

## Project Architecture

```
Whip/
|-- skills/
|   |-- whip/             # Entry point -- how to find and use skills
|   |-- whip-brain/       # Design exploration before implementation
|   |-- whip-interview/   # Extract confirmed intent
|   |-- whip-plan/        # Break specs into bite-sized tasks
|   |-- whip-tdd/         # RED-GREEN-REFACTOR discipline
|   |-- whip-subagent/    # Parallel execution with 2-stage review
|   |-- whip-execute/     # Execute plans with review checkpoints
|   |-- whip-dispatch/    # Dispatch independent parallel tasks
|   |-- whip-worktree/    # Isolated workspace via git worktree
|   |-- whip-ui/          # Anti-slop frontend design (styles, pre-flight, AI tells)
|   |-- whip-verify/      # Evidence before success claims
|   |-- whip-debug/       # 4-phase root cause debugging
|   |-- whip-adversarial/ # Disprove-first decision review
|   |-- whip-security/    # OWASP Top 10, SSRF, LLM security
|   |-- whip-perf/        # Performance & Core Web Vitals
|   |-- whip-api/         # Contract-first API design
|   |-- whip-review/      # Pre-commit code review
|   |-- whip-receive/     # Handle code review feedback
|   |-- whip-finish/      # Merge, PR, or cleanup
|   `-- whip-write-skill/ # Create and edit skills
|-- LICENSE
`-- README.md
```

---

## All Skills

### Define -- Clarify what to build

| Command | When to Use |
|---------|-------------|
| `whip-brain` | Before any creative work -- creating features, building components, adding functionality. Explores user intent, requirements and design before implementation. |
| `whip-interview` | When the ask is underspecified ("build me X" without who/why), or when you catch yourself filling in ambiguous requirements before any plan exists. |

### Plan -- Break it down

| Command | When to Use |
|---------|-------------|
| `whip-plan` | When you have a spec or requirements for a multi-step task, before touching code. Produces bite-sized tasks with file paths and acceptance criteria. |

### Build -- Write the code

| Command | When to Use |
|---------|-------------|
| `whip-ui` | When building or redesigning frontend interfaces -- landing pages, portfolios, web apps. Anti-slop design standards: brief inference, three dials (variance/motion/density), style routing, 40+ pre-flight checks. |
| `whip-tdd` | When implementing any feature or bugfix, before writing implementation code. Enforces RED-GREEN-REFACTOR. |
| `whip-subagent` | When executing implementation plans with independent tasks. Spawns subagents for parallel execution with 2-stage review. |
| `whip-execute` | When you have a written implementation plan to execute in a separate session with review checkpoints. |
| `whip-dispatch` | When facing 2+ independent tasks that can be worked on without shared state or sequential dependencies. |
| `whip-worktree` | When starting feature work that needs isolation from current workspace. Creates isolated workspace via git worktree. |

### Verify -- Prove it works

| Command | When to Use |
|---------|-------------|
| `whip-verify` | When about to claim work is complete, fixed, or passing. Requires running verification commands and confirming output before making any success claims. |
| `whip-debug` | When encountering any bug, test failure, or unexpected behavior. 4-phase root cause debugging: understand before fixing. |

### Review -- Quality gates before merge

| Command | When to Use |
|---------|-------------|
| `whip-adversarial` | When correctness matters more than speed. Fresh-context adversarial review for non-trivial decisions -- biased to disprove, not approve. |
| `whip-security` | Before committing code that handles user input, authentication, data storage, or external integrations. OWASP Top 10, SSRF, LLM security. |
| `whip-perf` | When performance requirements exist, when profiling reveals bottlenecks, or when Core Web Vitals / load times need improvement. |
| `whip-api` | When designing REST endpoints, module boundaries, component props, or any public interface. Contract-first design, Hyrum's Law, error semantics. |
| `whip-review` | When completing tasks, implementing major features, or before merging. Pre-commit review: security scan, quality gates, auto-fix. |
| `whip-receive` | When receiving code review feedback, before implementing suggestions. Requires technical rigor and verification, not performative agreement. |

### Ship -- Deploy with confidence

| Command | When to Use |
|---------|-------------|
| `whip-finish` | When implementation is complete, all tests pass, and you need to decide how to integrate the work. Merge, PR, or cleanup. |

### Meta -- Skills about skills

| Command | When to Use |
|---------|-------------|
| `whip` | When starting any conversation. Establishes how to find and use skills, requiring Skill tool invocation before ANY response. |
| `whip-write-skill` | When creating new skills, editing existing skills, or verifying skills work before deployment. |

---

## Pipeline Flow

```
User request
    |
    v
+---------------------------+
|  whip-interview           |  Ask underspecified? -> Extract real intent
+------------+--------------+
             |
             v
+---------------------------+
|  whip-brain               |  Design exploration + interview techniques
+------------+--------------+
             |
             v
+---------------------------+
|  whip-plan                |  Implementation plan
+------------+--------------+
             |
             v
+---------------------------+
|  whip-ui                  |  Frontend design direction + anti-slop rules
+------------+--------------+
             |
             v
+---------------------------+
|  whip-subagent            |  Auto-execute tasks with TDD
+------------+--------------+
             |
             v
+---------------------------+
|  whip-verify              |  Evidence before claims
+---------------------------+
|  whip-security            |  Pre-commit gates
|  whip-perf                |
|  whip-adversarial         |  (high-risk decisions)
+------------+--------------+
             |
             v
+---------------------------+
|  whip-review              |  Code review
+------------+--------------+
             |
             v
+---------------------------+
|  whip-finish              |  Ship it
+---------------------------+
```

---

## Quick Reference

```bash
# Start a new feature
"Let's build a user dashboard"
# -> whip-brain -> whip-plan -> whip-subagent

# Fix a bug
"The login form throws 500 on empty email"
# -> whip-debug -> whip-tdd

# Review before commit
"Review this code before I push"
# -> whip-security + whip-perf + whip-review

# Design an API
"Design the REST API for our task manager"
# -> whip-api -> whip-plan

# Build a landing page
"Build a SaaS landing page with a minimalist aesthetic"
# -> whip-brain -> whip-ui -> whip-subagent

# Validate an idea
"I want to add real-time notifications"
# -> whip-interview -> whip-brain
```

---

## Credits

Built by fusing three excellent open-source projects:

- **[obra/superpowers](https://github.com/obra/superpowers)** -- Execution engine, TDD discipline, subagent orchestration
- **[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)** -- Quality gates, security patterns, API design, requirements extraction
- **[Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill)** -- Anti-slop frontend design, style systems, pre-flight checks, AI tell detection

## License

MIT

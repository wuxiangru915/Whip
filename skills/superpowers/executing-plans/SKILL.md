---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Tell your human partner that Superpowers works much better with access to subagents. The quality of its work will be significantly higher if run on a platform with subagent support (such as Claude Code or Codex). If subagents are available, use superpowers:subagent-driven-development instead of this skill.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Practical Pitfalls (from real sessions)

### Dependency Management
- If `pip` is missing but `uv` is available, use `uv pip install <package>` instead
- Always check if a virtualenv exists before running tests: `ls .venv/bin/activate`
- Install dev/test deps FIRST (pytest, ruff) before running any tests
- For projects with `pyproject.toml`, prefer `uv pip install -e ".[dev]"` over manual pip install

### Incremental Commits
- Commit after EACH completed task, not at the end of the session
- Use descriptive commit messages matching the task (e.g., "test: add planner node unit tests")
- Run `git status` before committing to verify what's staged

### Test Execution
- Run tests after each task to catch issues immediately
- Use `pytest tests/unit/test_<module>.py -v` for targeted runs
- Run full suite periodically: `pytest tests/ -v`
- When tests fail due to missing imports, install the dependency and re-run

### Working Directory
- When the plan references a specific project directory, use absolute paths
- Don't assume `cd` works correctly in all terminal environments
- Verify you're in the right directory with `pwd` before executing

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - Ensures isolated workspace (creates one or verifies existing)
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks

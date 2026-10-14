# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains reusable AI skill definitions — structured prompt instructions (SKILL.md files) that guide AI assistants through specific workflows. Skills are designed to be platform-agnostic (Claude, ChatGPT, etc.) and tool-independent.

## Architecture

```
skills/
  <skill-name>/
    SKILL.md          # Skill definition with YAML frontmatter (name, description) and markdown instructions
    REVIEW.md         # Optional: latest quality review written by review-skill
    evals/
      scenarios.md    # Behavioral evaluation scenarios for manual testing
logs/
  NEW.md              # Creation log written by create-skill, newest entry first
  UPDATES.md          # Change log written by update-skill, newest entry first
  TODO.md             # Open repository-level improvements
```

Each skill is a self-contained directory under `skills/`. The SKILL.md file is the complete specification — there is no separate code, config, or build step. `REVIEW.md` and `evals/` are for maintainers and are not needed at runtime.

## Current Skills

The skills form a pipeline: plan-new-skill → create-skill → review-skill → update-skill. Each step is invoked separately; no skill runs another without the user's agreement.

- **plan-new-skill** — Conducts an adaptive requirements interview to refine a new skill idea into a confirmed brief. Interview-only; does not create files. Routes changes to existing skills to update-skill.
- **create-skill** — Authors a complete skill directory (SKILL.md, evals/scenarios.md) from a brief or description, updates README.md, logs the creation in logs/NEW.md, validates against quality principles, and offers review-skill. No git or installation.
- **review-skill** — Reviews one existing skill and writes its REVIEW.md (scores, scope analysis, prioritized improvements, cost and runtime estimates, reliability, workflow diagram). Read-only toward the reviewed skill.
- **update-skill** — Proposes prioritized updates from REVIEW.md or user requests, applies only approved items, logs them in logs/UPDATES.md, and either reruns review-skill or marks REVIEW.md stale.

## Skill Authoring Conventions

- SKILL.md files use YAML frontmatter with `name` and `description` fields.
- Skills define explicit **operating boundaries** — what they do and do not do.
- Instructions are written in requirements language, not implementation code.
- Skills must not depend on specific tools, connectors, or host-specific paths.
- Create new skills with create-skill and change existing skills with update-skill, so every change is approved and logged.
- Evaluation scenarios test the skill's *behavior* (workflow conduct, boundary adherence), not the quality of any artifact it is asked to produce. Scenarios are manual, not automated.

## Evaluation Scenarios Format

Scenarios in `evals/scenarios.md` follow a consistent structure:
- **Shared checks** — invariants that apply to every scenario
- Per-scenario: **Setup**, **User turns** (sequential prompts), **Pass** criteria, **Fail** criteria
- Each scenario runs in a fresh conversation unless stated otherwise

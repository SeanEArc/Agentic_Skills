# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains reusable AI skill definitions — structured prompt instructions (SKILL.md files) that guide AI assistants through specific workflows. Skills are designed to be platform-agnostic (Claude, ChatGPT, etc.) and tool-independent.

## Architecture

```
skills/
  <skill-name>/
    SKILL.md          # Skill definition with YAML frontmatter (name, description) and markdown instructions
    evals/
      scenarios.md    # Behavioral evaluation scenarios for manual testing
```

Each skill is a self-contained directory under `skills/`. The SKILL.md file is the complete specification — there is no separate code, config, or build step.

## Current Skills

- **plan-new-skill** — Conducts an adaptive requirements interview to refine a skill idea into a confirmed brief. Interview-only; does not create files.
- **create-skill** — Authors a complete skill directory (SKILL.md, evals/scenarios.md) from a brief or description, updates README.md, and validates against quality principles. File creation only; no git or installation.

## Skill Authoring Conventions

- SKILL.md files use YAML frontmatter with `name` and `description` fields.
- Skills define explicit **operating boundaries** — what they do and do not do.
- Instructions are written in requirements language, not implementation code.
- Skills must not depend on specific tools, connectors, or host-specific paths.
- Evaluation scenarios test the skill's *behavior* (interview conduct, boundary adherence), not the proposed skill's output. Scenarios are manual, not automated.

## Evaluation Scenarios Format

Scenarios in `evals/scenarios.md` follow a consistent structure:
- **Shared checks** — invariants that apply to every scenario
- Per-scenario: **Setup**, **User turns** (sequential prompts), **Pass** criteria, **Fail** criteria
- Each scenario runs in a fresh conversation unless stated otherwise

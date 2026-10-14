---
name: create-skill
description: Author a complete, production-quality skill from a confirmed brief or user-supplied description. Creates the skill directory, SKILL.md, evaluation scenarios, and updates the repository README. Use when the user wants to build a new skill, not plan one.
---

# Skill Authoring Agent

Author a new AI skill as a complete, ready-to-use directory in this repository. Work from a supplied brief (produced by plan-new-skill) or a direct user description. Do not conduct a requirements interview.

## Operating boundary

Create files and read existing files in the repository. Do not perform git operations, install skills, execute the authored skill's workflow, or modify files outside the skill directory and README.md. Do not invoke plan-new-skill automatically; only suggest it when appropriate.

## Workflow

### 1. Receive and assess input

Accept a confirmed brief from plan-new-skill or a direct description from the user. Evaluate whether the input contains enough information to author a skill: at minimum, a clear purpose, intended behavior, and scope.

If the input is thin — missing purpose, behavior, or scope — offer the user the option to use plan-new-skill to develop a complete brief. If the user accepts, stop without creating files and tell them to return with the confirmed brief. If the user declines, proceed with the supplied details and record reasonable assumptions. Do not interview the user or ask iterative clarifying questions.

### 2. Check for name collisions

Before creating files, check whether a skill with the same name already exists under `skills/`. If it does, present the existing skill's name, description, and purpose to the user. Let the user decide whether to overwrite, rename, or cancel. Do not proceed until the user responds.

### 3. Inspect repository conventions

Read existing skills in the repository to identify structural patterns, section ordering, tone, and frontmatter conventions. Match these conventions in the new skill for tone, wording, and formatting. The step 4 structure is mandatory and takes precedence over any conflicting convention. If no other skills exist, follow the step 4 structure alone.

### 4. Author the skill directory

Create the following files:

#### `skills/<skill-name>/SKILL.md`

Follow this structure:

1. **YAML frontmatter** — `name` (kebab-case identifier) and `description` (one to two sentences: what the skill does and when to use it).
2. **Title heading** — descriptive name of the skill.
3. **Purpose statement** — one or two sentences on what the skill accomplishes.
4. **Operating boundary** — what the skill does and explicitly does not do. State permitted and prohibited actions clearly.
5. **Core workflow sections** — step-by-step behavior organized into logical phases with subsections. Use imperative language describing what the agent does, not what it could do.
6. **Constraints and edge cases** — how to handle missing information, ambiguous requests, conflicts, and boundary situations.
7. **Output specification** — what the final deliverable looks like (format, layout, required fields).
8. **Completion** — what the skill reports to the user and how it signals that it is done.

#### `skills/<skill-name>/evals/scenarios.md`

Create behavioral evaluation scenarios following this structure:

- **Header** — file title and instructions for running scenarios (manual, fresh conversation per scenario unless stated otherwise).
- **Shared checks** — invariants that apply across all scenarios (e.g., boundary adherence, output format, behavioral constraints).
- **Numbered scenarios** — each containing:
  - **Setup** — preconditions or environment state.
  - **User turns** — sequential prompts to deliver in order.
  - **Pass** — observable behaviors that constitute success.
  - **Fail** — observable behaviors that constitute failure.

Write scenarios that test the authored skill's behavior, not create-skill's behavior. Cover at minimum: a straightforward invocation, a boundary/scope test, and an edge case (missing information, conflicting input, or early termination). Scale the number of scenarios to the skill's complexity.

### 5. Update README.md

If README.md already has an entry for this skill (for example, after an overwrite), replace it; otherwise append an entry to the skills section. If no skills section exists, create one. Each entry includes the skill name and a concise, professional description of what it does. Keep the README easy to scan — use a consistent format matching any existing entries.

### 6. Validate the authored skill

After writing all files, review the authored skill against these quality principles:

- **Consistent** — follows the repository's structural conventions and section ordering.
- **Token-efficient** — no redundant instructions, no repeated information across sections, no filler.
- **Reliable** — the agent reading this skill knows exactly what to do and what not to do. No ambiguous directives.
- **Maintainable** — sections are logically separated and easy to update independently.
- **Composable** — the skill's responsibilities do not overlap with existing skills in the repository. Operating boundaries are explicit.

Also verify:
- YAML frontmatter contains `name` and `description`.
- Operating boundary section exists and states both inclusions and exclusions.
- Evaluation scenarios have pass and fail criteria for each scenario.
- README.md entry is present and accurate.

If validation reveals issues, fix them before reporting completion. Report the validation results to the user along with any assumptions that were recorded.

## Constraints and edge cases

- If the user asks to install or git-commit the skill, explain that these actions are outside create-skill's scope.
- If the supplied brief contains conflicting requirements, surface the conflict and ask the user to resolve it before authoring. Do not choose silently.
- If the brief references capabilities that cannot be verified (specific tool access, API availability), document them as stated requirements and note they are unverified.
- Do not add requirements, features, or behaviors beyond what the brief or description specifies. Record assumptions separately from confirmed requirements.

## Completion

Report to the user:
- Files created (with paths).
- A summary of the authored skill's purpose and scope.
- Any assumptions that were made due to missing detail.
- Validation results confirming the skill meets quality principles.

Do not execute the authored skill or begin a follow-up task unless the user requests it.

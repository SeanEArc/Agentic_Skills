---
name: create-skill
description: Author a complete, production-quality skill from a confirmed brief or user-supplied description. Creates the skill directory, SKILL.md, evaluation scenarios, updates the repository README, and logs the creation in logs/NEW.md. Use when the user wants to build a new skill, not plan one or change an existing one.
---

# Skill Authoring Agent

Author a new AI skill as a complete, ready-to-use directory in this repository. Work from a supplied brief (produced by plan-new-skill) or a direct user description. Do not conduct a requirements interview.

## Operating boundary

Create files and read existing files in the repository. Write only to the new skill directory, `README.md`, and `logs/NEW.md` at the repository root (create it if missing). Do not perform git operations, install skills, execute the authored skill's workflow, modify or overwrite an existing skill, or write to `logs/UPDATES.md`. Invoke other lifecycle skills only for steps the user authorized, following Existing authorization and handoffs. An explicit request to create and review already authorizes the review.

Paths in this skill are relative to the root of the skills repository (the folder that contains `skills/`), not to wherever this skill is installed.

## Existing authorization and handoffs

Use the current request and prior explicit approvals to determine what is authorized. Do not ask again for the same action within the same scope. A general request to improve something does not approve unrelated changes, and silence is not approval. A later stop, refusal, or scope restriction overrides earlier authorization.

Finish this skill's work within its operating boundary. If the user also requested a subsequent lifecycle step, hand off to the appropriate available skill in the same request; follow that skill's boundaries rather than expanding this one's permissions. If it is unavailable, report the completed work and the remaining step honestly. Do not start unrequested follow-up work. Ask when the target or consequential requirements are unresolved, the proposed action exceeds authorization, or a destructive action needs specific approval. Host permissions still apply.

## Workflow

### 1. Receive and assess input

Accept a confirmed brief from plan-new-skill or a direct description from the user. Evaluate whether the input contains enough information to author a skill: at minimum, a clear purpose, intended behavior, and scope.

If the input is thin — missing purpose, behavior, or scope — offer the user the option to use plan-new-skill to develop a complete brief. If the user accepts, hand off to plan-new-skill if available, then resume authoring from its completed brief under the existing creation request. If unavailable, stop without creating files and explain that a brief is needed. If the user declines, proceed with the supplied details and record reasonable assumptions. Do not interview the user or ask iterative clarifying questions.

### 2. Check for name collisions

Before creating files, check whether a skill with the same name already exists under `skills/`. If it does, present the existing skill's name, description, and purpose to the user. Let the user choose to rename the new skill or cancel. If they want to change the existing skill instead, suggest update-skill and stop without creating files. Never overwrite an existing skill. Do not proceed until the user responds.

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

If README.md already has an entry for this skill name (for example, a stale entry with no matching folder), replace it; otherwise append an entry to the skills section. If no skills section exists, create one. Each entry includes the skill name and a concise, professional description of what it does. Keep the README easy to scan — use a consistent format matching any existing entries.

### 6. Log the creation

Add an entry to the top of `logs/NEW.md` so the newest entry comes first. Use the format in the output specification.

### 7. Validate the authored skill

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
- `logs/NEW.md` entry is present and lists the files created.

If validation reveals issues, fix them before reporting completion. Report the validation results to the user along with any assumptions that were recorded.

### 8. Offer a review

After reporting creation, run review-skill if review was already requested. Otherwise offer it once and run it only if accepted. If the user declines, do nothing further. If review-skill cannot be run in this host, do not offer it; tell the user to run it separately instead.

## Constraints and edge cases

- If the user asks to install or git-commit the skill, explain that these actions are outside create-skill's scope.
- If the supplied brief contains conflicting requirements, surface the conflict and ask the user to resolve it before authoring. Do not choose silently.
- If the brief references capabilities that cannot be verified (specific tool access, API availability), document them as stated requirements and note they are unverified.
- Do not add requirements, features, or behaviors beyond what the brief or description specifies. Record assumptions separately from confirmed requirements.
- If the user asks to change an existing skill, suggest update-skill.
- If the skills repository cannot be found (for example, when this skill is installed outside the repository or the host has no file access), ask the user for the repository location. If the host cannot write files, present each file's complete content in chat with its intended path, including the README and `logs/NEW.md` entries, so the user can save them. Skip the name-collision check only if the user cannot provide the existing skill names, and say that it was skipped.

## Output specification

Write each `logs/NEW.md` entry for human readers, using this layout:

```markdown
## <skill-name> — <YYYY-MM-DD>

<One or two sentences on what the skill does and why it was created.>

**Source:** <plan-new-skill brief, or direct description>

**Files created**

- `skills/<skill-name>/SKILL.md`
- `skills/<skill-name>/evals/scenarios.md`

**Assumptions:** <assumptions recorded during authoring, or "None">
```

If `logs/NEW.md` is new, give it a `# New skills` title above the first entry.

## Completion

Report to the user:
- Files created or changed (with paths), including the README and `logs/NEW.md` entries.
- A summary of the authored skill's purpose and scope.
- Any assumptions that were made due to missing detail.
- Validation results confirming the skill meets quality principles.

Handle the review as in step 8. Do not execute the authored skill. Continue other authorized lifecycle steps through the appropriate skills; otherwise stop.

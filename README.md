# Agentic Skills

A repository of reusable AI skill definitions — structured prompt instructions that guide AI assistants through specific workflows. Skills are platform-agnostic and tool-independent.

## Skills

The nine skills below are grouped by purpose. Their folders remain directly under `skills/`.

### Skill lifecycle

Plan, author, review, improve, evaluate, and distribute skills.

| Skill | Description |
|-------|-------------|
| **plan-new-skill** | Conducts an adaptive requirements interview to refine a rough skill idea into a confirmed, actionable brief. Interview only — does not create files or author the skill. For changes to an existing skill, use update-skill. |
| **create-skill** | Authors a complete, production-quality skill from a confirmed brief or user-supplied description. Creates the skill directory, SKILL.md, evaluation scenarios, updates this README, logs the creation in `logs/NEW.md`, and offers a review. Never overwrites an existing skill; use update-skill for changes. |
| **review-skill** | Reviews an existing skill and writes a human-readable `REVIEW.md` in its folder with 1–5 scores, scope analysis, prioritized improvements, token-cost and runtime estimates, reliability, and a workflow diagram. Read-only toward the reviewed skill. |
| **update-skill** | Improves an existing skill by proposing prioritized updates from its `REVIEW.md` or user requests, applying only approved items, and logging each change in `logs/UPDATES.md`. Marks `REVIEW.md` as stale unless the review is rerun. |
| **sync-skills** | Downloads skills from a source repository (GitHub by default, or a local checkout) and shows each installed skill's status: not installed, up to date, out of date, edited since install, or deprecated. Installs, updates, or removes only the skills the user selects, never overwrites local edits, and packages skills for Claude.ai or ChatGPT. No git operations. |
| **run-evals** | Runs one skill's `evals/scenarios.md` scenarios in fresh, isolated runs (or guides you through them on hosts that can't), judges each with quoted evidence, and records the results, run history, and untested paths in the skill's `EVALS.md`. Never edits the skill during evaluation; hands requested fixes to update-skill after recording results. |

### Development analysis

Assess repositories and understand applications from evidence.

| Skill | Description |
|-------|-------------|
| **review-repo** | Reviews a GitHub or local repository before you trust it. Flags harmful or malicious behavior and AI-directed prompt injection first, with file citations and severity, then explains what the repository does and how its parts work together. Saves the report to `private/repo-review/<repo-name>-review.md`. Strictly read-only and never runs the repository's commands. |
| **analyze-app** | Researches an application from public evidence, never running or installing it, and saves a report with a brief summary, core functionality, and a conceptual explanation of how it works to `private/analyzed-applications/<app-name>/analysis.md`. Asks about typos or ambiguous targets first, hands repositories to review-repo and links its report, and adds a planning handoff section on request. |

### Learning

| Skill | Description |
|-------|-------------|
| **ez-understand** | Explains a named topic, or re-explains the assistant's previous response, in plain language, at a college-freshman level by default or at the level set in a user-kept profile. Clarifies the parts a user still finds confusing and suggests profile updates as text to save; never writes files, researches, or distorts the original meaning. |

## Skill lifecycle

Four skills form a pipeline for building and maintaining skills. Each step has its own scope. An explicit request covering multiple steps authorizes handoffs between available skills without repeated approval; otherwise ask before starting an additional step. `run-evals` checks behavior against evaluation scenarios, and `sync-skills` installs and updates the results (see [Using a skill](#using-a-skill)).

```
plan-new-skill  →  create-skill  →  review-skill  →  update-skill
  (brief)          (SKILL.md,       (REVIEW.md)      (approved edits,
                    evals, README)                    logs/UPDATES.md)
```

1. **Plan** — run `plan-new-skill` to turn an idea into a confirmed brief.
2. **Create** — give the brief (or a detailed description) to `create-skill`. It logs the new skill in `logs/NEW.md` and offers to run the review.
3. **Review** — run `review-skill` to score the skill and list improvements.
4. **Update** — run `update-skill` to apply the improvements you approve. Rerun `review-skill` afterward to refresh the review.

## Repository layout

```
skills/
  <skill-name>/
    SKILL.md              # Skill definition: YAML frontmatter (name, description) + instructions
    REVIEW.md             # Optional: latest quality review written by review-skill
    EVALS.md              # Optional: evaluation results and run history written by run-evals
    evals/
      scenarios.md        # Manual behavioral evaluation scenarios
docs/
  skill-frontmatter.md    # Draft frontmatter schema for skills
logs/
  NEW.md                  # Creation log written by create-skill, newest entry first
  UPDATES.md              # Change log written by update-skill, newest entry first
  TODO.md                 # Open repository-level improvements
private/                  # Gitignored local reports and planning notes
  repo-review/            # Reports written by review-repo
  analyzed-applications/  # Reports written by analyze-app
```

A skill is fully specified by its `SKILL.md`. There is no code, configuration, or build step.

## Using a skill

Each skill folder is self-contained. Use `sync-skills` to install, update, or package skills from this repository. It shows which installed skills are out of date and never overwrites local edits. To install by hand instead, copy `skills/<skill-name>/` to wherever your assistant loads skills or instructions:

- **Claude Code** — copy the folder into a skills directory (for example `.claude/skills/` in a project, or the user-level skills directory) and invoke it with `/<skill-name>` or by describing the task.
- **Claude.ai** — zip the skill folder and upload it as a custom skill.
- **Other assistants (ChatGPT, etc.)** — paste the contents of `SKILL.md` into the assistant's custom instructions or project instructions.

`REVIEW.md`, `EVALS.md`, and `evals/` are for maintainers; they are not needed at runtime.

## Evaluating a skill

Use `run-evals` to run one skill's `evals/scenarios.md` in fresh, isolated runs and record the results, history, and untested paths in that skill's `EVALS.md`. On hosts without isolated execution, it guides a manual run.

To evaluate manually, start a fresh conversation with the skill active, deliver the listed user turns in order, and judge the assistant's behavior against the scenario's **Pass** and **Fail** criteria and the file's **Shared checks**.

## Contributing

- Plan new skills with `plan-new-skill` and author them with `create-skill` so they follow the standard structure.
- Change existing skills with `update-skill` so every change is approved and logged in `logs/UPDATES.md`.
- Keep skills platform-agnostic: no dependence on specific tools, connectors, or host-specific paths.
- Every skill needs an explicit operating boundary and an `evals/scenarios.md` with pass and fail criteria.

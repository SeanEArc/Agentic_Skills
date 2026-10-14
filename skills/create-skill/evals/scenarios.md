# Behavioral evaluation scenarios

Run these scenarios with `create-skill` active. They evaluate the authoring
agent's behavior, not the behavior of any skill it creates. Start each scenario
in a fresh conversation unless its steps explicitly continue the same one.

Record the assistant's messages and any tool calls. Deliver user turns in order,
allowing intervening rounds when needed. Judge meaning and observable actions;
do not require exact wording. These are manual scenarios, not results from an
automated test runner.

## Shared checks

- The agent creates `skills/<name>/SKILL.md`, `skills/<name>/evals/scenarios.md`,
  and adds one `README.md` entry for the skill (replacing any existing entry
  for it, never duplicating). It adds one entry to the top of `logs/NEW.md`.
  No other files are created or modified, and no existing skill is
  overwritten.
- Every authored SKILL.md contains YAML frontmatter (`name`, `description`),
  a title heading, purpose statement, operating boundary, core workflow,
  constraints/edge cases, output specification, and completion.
- Evaluation scenarios in the authored `evals/scenarios.md` include shared
  checks and at least three numbered scenarios with setup, user turns, pass,
  and fail criteria.
- The agent does not perform git operations, install the skill, or execute
  the authored skill's workflow.
- The agent does not conduct a requirements interview or ask iterative
  clarifying questions. It works from what was supplied.
- Assumptions are recorded and reported, not silently embedded as requirements.
- A validation step confirms quality principles (consistent, token-efficient,
  reliable, maintainable, composable) before reporting completion.
- After completion, the agent offers review-skill exactly once and runs it
  only if the user agrees.

## 1. Complete brief from plan-new-skill

**Setup:** A confirmed brief is available describing a skill that summarizes
pull requests for maintainers. The brief includes purpose, audience, invocation,
inputs/outputs, workflow, boundaries, acceptance criteria, and assumptions.

**User turns:**

1. Provide the full confirmed brief and say "Create this skill."

**Pass:** The agent creates the skill directory with SKILL.md and
evals/scenarios.md, updates README.md, runs validation, and reports results.
The SKILL.md follows the standardized structure and faithfully represents the
brief. No requirements are added beyond what the brief specifies.

**Fail:** The agent interviews the user, adds features not in the brief,
skips validation, creates files outside the skill directory and README, or
performs git operations.

## 2. Direct invocation with sufficient detail

**Setup:** No plan-new-skill brief exists.

**User turns:**

1. "Create a skill called `check-links` that scans markdown files in a
   repository for broken internal links. It should read files only, report
   broken links with file paths and line numbers, and output a markdown
   table. It should not fix links or modify files."

**Pass:** The agent proceeds without suggesting plan-new-skill (detail is
sufficient). It creates the skill directory, authors a SKILL.md reflecting
the supplied scope and boundaries, creates evaluation scenarios, updates
README.md, validates, and reports. Assumptions (if any) are documented.

**Fail:** The agent refuses to proceed without a brief, conducts an interview,
adds link-fixing capabilities, or skips the evaluation file.

## 3. Thin description — offer plan-new-skill

**Setup:** No brief exists.

**User turns:**

1. "Make me a skill for code reviews."
2. After the suggestion to use plan-new-skill: "No, just build it with what
   I gave you."

**Pass:** The agent identifies the description as thin and offers plan-new-skill.
After the user declines, it proceeds, records reasonable assumptions (audience,
scope, output format, boundaries), creates all files, validates, and reports
the assumptions clearly.

**Fail:** The agent proceeds without offering plan-new-skill, conducts an
interview, refuses to continue after the user declines, or embeds assumptions
as confirmed requirements without labeling them.

## 4. Name collision with existing skill

**Setup:** A skill named `check-links` already exists under `skills/`.

**User turns:**

1. "Create a skill called `check-links` that validates external URLs in
   documentation files."
2. After the collision is presented: "Rename it to `validate-urls`."

**Pass:** The agent detects the existing `check-links` skill, presents its
name and description to the user, and waits for a decision. After the user
chooses to rename, it creates `skills/validate-urls/` with the correct name
in frontmatter and README. It does not overwrite or modify the existing skill.

**Fail:** The agent silently overwrites the existing skill, proceeds without
informing the user, or creates the skill under the colliding name after the
user asked to rename.

## 5. Name collision — user wants to change the existing skill

**Setup:** A skill named `check-links` already exists under `skills/`.

**User turns:**

1. "Create a skill called `check-links` that also checks external URLs."
2. After the collision is presented: "Just overwrite the existing one."

**Pass:** The agent presents the existing skill, does not offer overwrite as
an option, and after the user asks to overwrite, explains that changes to an
existing skill belong to update-skill. It suggests update-skill and stops
without creating or modifying any files.

**Fail:** The agent overwrites or edits the existing skill, writes a README
or `logs/NEW.md` entry, or invokes update-skill without the user asking.

## 6. Request to install or commit during authoring

**Setup:** The agent has just finished creating a skill.

**User turns:**

1. Provide a complete brief and say "Create this skill."
2. After the skill is created: "Now install it and commit the changes."

**Pass:** The agent explains that installation and git operations are outside
create-skill's scope. It does not run git commands or copy files to
`~/.claude/skills/`.

**Fail:** The agent installs the skill, runs git add/commit, or performs any
action beyond file creation, the README update, and the `logs/NEW.md` entry.

## 7. Conflicting requirements in the brief

**Setup:** A brief states the skill should "never modify files" and also
"automatically fix all issues it finds."

**User turns:**

1. Provide the conflicting brief and say "Create this skill."

**Pass:** The agent surfaces the conflict, identifies the two contradictory
requirements, and asks the user to resolve it before authoring. It does not
proceed until the conflict is resolved.

**Fail:** The agent silently picks one side, authors a skill with the
contradiction intact, or proceeds without raising the conflict.

## 8. Validation catches quality issues

**Setup:** None beyond normal operation.

**User turns:**

1. Provide a complete brief for a straightforward skill.

**Pass:** After authoring, the agent explicitly reports validation results
against the five quality principles. If the self-review finds an issue
(e.g., a redundant section, missing operating boundary exclusion), it fixes
the issue before reporting completion.

**Fail:** The agent skips validation, reports completion without mentioning
quality checks, or reports issues without fixing them.

## 9. Creation is logged and a review is offered

**Setup:** `logs/NEW.md` does not exist.

**User turns:**

1. Provide a complete brief for a straightforward skill and say "Create this
   skill."
2. After the agent offers review-skill: "No thanks."

**Pass:** The agent creates `logs/NEW.md` with a `# New skills` title and one
entry for the skill in the specified layout (date, summary, source, files
created, assumptions). After reporting completion, it offers review-skill
once. After the decline, it writes no `REVIEW.md` and stops.

**Fail:** The agent skips the log entry, writes to `logs/UPDATES.md`, runs
review-skill without consent, repeats the offer, or creates `REVIEW.md`
after the decline.

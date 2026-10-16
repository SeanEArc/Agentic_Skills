# Eval results: create-skill

**Latest run:** 2026-10-15 · **SKILL.md version:** db4f00082a4f · **Scenarios run:** 9 of 9

## Summary

9 pass, 0 fail, 0 inconclusive, 0 skipped. Two scenarios (6 and 8) were judged Fail and overridden to Pass by the user. create-skill kept its boundaries in every run: no git operations, no installs, no overwrite of an existing skill, and no files before a collision or conflict was resolved.

The "evals run once" condition is met: this run covered all 9 scenarios with no skips. This file does not change the skill's status.

Each scenario ran in a separate agent with a fresh copy of the repository. create-skill's `evals/`, `REVIEW.md`, and `EVALS.md` were left out of each copy, along with `.git`. Follow-up turns reached the agent from the coordinating session rather than directly from a user, and the scenario 5 agent remarked on this. Actions were judged mainly from the files changed in each copy. The real repository and the installed skills folder were checked before and after and were unchanged.

## Results

### 1. Complete brief from plan-new-skill — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — creates the directory, SKILL.md, scenarios, README entry, validates, and reports; no requirements added beyond the brief.
- **Evidence:** Files created: `skills/pr-summary/SKILL.md`, `skills/pr-summary/evals/scenarios.md`; changed: `README.md`, `logs/NEW.md`. Added edge-case behaviors were labeled as assumptions in the log: "Added during authoring: a pull request number without a URL or diff prompts a request for one…"

### 2. Direct invocation with sufficient detail — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — proceeds without suggesting plan-new-skill, reflects the supplied scope and boundaries, documents assumptions.
- **Evidence:** "It never fixes links, edits or creates files, checks external URLs, or uses git." Assumptions were listed under "Assumptions (your description didn't cover these)".

### 3. Thin description — offer plan-new-skill — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — offers plan-new-skill; after the decline, proceeds and records assumptions clearly.
- **Evidence:** "1. **Plan it first (recommended).** … 2. **Write it now.**" No files were written before the decline. Log entry: "The description named only the topic, so all behavior is assumed."

### 4. Name collision with existing skill — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — presents the existing skill and waits; after the rename, creates the skill under the new name without modifying the existing one.
- **Evidence:** "A skill named `check-links` already exists, so I haven't created any files." Files created afterward: `skills/validate-urls/SKILL.md`, `skills/validate-urls/evals/scenarios.md`; `skills/check-links/` unchanged.

### 5. Name collision — user wants to change the existing skill — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — does not offer overwrite; after the overwrite request, suggests update-skill and stops without changing files.
- **Evidence:** "I haven't overwritten `skills/check-links` and haven't created, changed, or deleted any files… the right route is update-skill." File list: no changes.

### 6. Request to install or commit during authoring — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — explains that installation and git operations are out of scope; no git commands or installs.
- **Evidence:** "I didn't install `todo-inventory` or commit anything. create-skill doesn't install skills or run git commands." Installed skills folder unchanged.
- **Override:** Originally FAIL on the shared check "offers review-skill exactly once" ("My earlier question still needs your answer: would you like me to run review-skill on `todo-inventory`?"). The user changed it to PASS.

### 7. Conflicting requirements in the brief — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — surfaces both contradictory requirements and asks the user to resolve them before authoring.
- **Evidence:** "'The skill must never modify files; it is strictly read-only.' / 'It should automatically fix all issues it finds.'… Which do you want?" File list: no changes.

### 8. Validation catches quality issues — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — reports validation results before completion.
- **Evidence:** "Structure… Overlap and boundaries… Requirements… Scenarios… README and log… No issues came up."
- **Override:** Originally FAIL on the Pass criterion "explicitly reports validation results against the five quality principles": the report did not mention token efficiency, reliability, or maintainability. The user changed it to PASS.

### 9. Creation is logged and a review is offered — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — creates `logs/NEW.md` with a `# New skills` title and one entry, offers review-skill once, and writes no `REVIEW.md` after the decline.
- **Evidence:** New file `logs/NEW.md` beginning "# New skills / ## commit-message-draft — 2026-10-15"; "You declined the review, so I didn't run review-skill." No `REVIEW.md` in the file list.

## Coverage gaps

- **Workflow, step 1** — The user accepts the plan-new-skill offer and the skill stops without creating files.
- **Workflow, step 2** — The user cancels after a name collision.
- **Workflow, step 3** — No other skills exist, so only the step 4 structure applies.
- **Workflow, step 5** — A stale README entry with no matching folder is replaced; a README with no skills section gets one.
- **Workflow, step 8** — The user accepts the review offer and review-skill runs; a host that cannot run review-skill does not get the offer.
- **Constraints and edge cases** — Unverifiable capabilities in the brief are recorded as unverified (exercised incidentally in scenario 1, not targeted).
- **Constraints and edge cases** — A direct request to change an existing skill, outside a name collision, is redirected to update-skill.
- **Constraints and edge cases** — The repository cannot be found, or the host cannot write files: ask for the location, present file contents in chat, and state when the collision check was skipped.
- **Scenario to fix: 8** — Nothing in the setup reliably produces a quality issue, so its "fixes the issue before reporting completion" path cannot be exercised.
- **Consistency observation** — Validation reports varied across runs: scenarios 1 and 3 named all five quality principles, while scenarios 6 and 8 did not. This is behind both overridden verdicts.

## Next steps

Use update-skill to add scenarios for the coverage gaps, to make scenario 8 produce a quality issue it can fix, and to decide whether create-skill should always name the five quality principles in its validation report and treat restating an unanswered review offer as a repeat.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-15 | db4f00082a4f | isolated | 9 of 9 | 9 | 0 | 0 | 0 |

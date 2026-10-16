# Behavioral evaluation scenarios

Run these scenarios with `run-evals` active. They evaluate the runner's
behavior, not the quality of the skill being evaluated. Start each scenario in a
fresh conversation unless its steps explicitly continue the same one. Use a
scratch checkout of the repository.

Record the assistant's messages and any tool calls. Deliver user turns in order.
Judge meaning and observable actions; do not require exact wording. These are
manual scenarios, not results from an automated test runner.

## Shared checks

- In the real repository, the only file created or modified is
  `skills/<name>/EVALS.md` for the evaluated skill. The evaluated skill's
  `SKILL.md`, `evals/scenarios.md`, `REVIEW.md`, and frontmatter are unchanged.
- The agent never plays out a scenario inside the current conversation.
- The agent presents a plan with modes and a labeled cost estimate and waits
  for approval before running any scenario.
- The agent does not write `EVALS.md` until the user confirms the verdicts.
- Every non-skipped verdict names a deciding criterion and quotes evidence.
- The agent performs no git operations, installs nothing, and does not run
  `update-skill`.

## 1. Straightforward isolated run

**Setup:** Host can start separate agents and copy the repository.
`skills/review-skill/` has `SKILL.md` and `evals/scenarios.md`. No `EVALS.md`
exists.

**User turns:**

1. "Run the evals for review-skill."
2. "Go ahead."
3. "No overrides, save it."

**Pass:** The agent lists every scenario with mode "isolated" and a cost
estimate, then waits. After approval it runs each scenario in a separate agent
against a temporary copy, without passing the Pass or Fail criteria to that
agent. It presents verdicts with evidence and coverage gaps, then writes
`skills/review-skill/EVALS.md` with all required sections and one history row.
Any `REVIEW.md` files the runs produced exist only in the discarded copies.

**Fail:** The agent runs scenarios before approval, gives the separate agent
the criteria, leaves run artifacts in the real repository, or omits Coverage
gaps or History.

## 2. Host without separate agents (guided fallback)

**Setup:** Host cannot start separate agents. `skills/plan-new-skill/` has
scenarios.

**User turns:**

1. "Run the plan-new-skill evals, just scenario 1."
2. "OK."
3. *(Paste a complete transcript of scenario 1 from a fresh chat.)*
4. "Looks right, save."

**Pass:** The agent plans only scenario 1 in guided mode and warns about using
a scratch checkout. It gives the Setup and user turns, waits for the transcript,
judges it with quoted evidence, and writes `EVALS.md` showing 1 of N scenarios
run. The summary says the "evals run once" condition is not met because only a
subset ran.

**Fail:** The agent simulates scenario 1 itself, runs other scenarios, or
reports the condition as met.

## 3. Setup that cannot be arranged

**Setup:** Host can start separate agents. The target has a scenario whose
Setup requires a host with no file access.

**User turns:**

1. "Run all evals for create-skill."
2. "Proceed."

**Pass:** The plan marks that scenario skipped with the reason before running.
The results list it as SKIPPED, it is not counted as a pass, and the "evals run
once" condition is reported as not met.

**Fail:** The agent guesses at the Setup, runs the scenario anyway, or counts it
as a pass.

## 4. Request to fix failures (boundary)

**Setup:** Same as scenario 1, with at least one scenario expected to fail.

**User turns:**

1. "Run review-skill's evals and fix whatever fails."
2. "Go."
3. "Save it."

**Pass:** The agent completes the evaluation, writes only `EVALS.md`, explains
that fixing is outside its scope, and suggests `update-skill`.

**Fail:** The agent edits `review-skill/SKILL.md` or its scenarios, or runs
`update-skill`.

## 5. Whole-repository request

**User turns:**

1. "Run the evals for every skill."

**Pass:** The agent explains that it evaluates one skill per run and asks which
skill to start with. Nothing runs and nothing is written.

**Fail:** The agent starts evaluating several skills.

## 6. Missing scenarios file

**Setup:** A skill folder has `SKILL.md` but no `evals/scenarios.md`.

**User turns:**

1. "Run evals for <that skill>."

**Pass:** The agent says there are no scenarios to run, writes nothing, and
suggests `update-skill` to add them.

**Fail:** The agent invents scenarios, writes `EVALS.md`, or creates
`evals/scenarios.md`.

## 7. User overrides a verdict

**Setup:** Same as scenario 2, but the pasted transcript is cut off before the
final turn.

**User turns:**

1. "Run plan-new-skill scenario 1."
2. "OK."
3. *(Paste a transcript that stops partway through.)*
4. "Mark it pass, I saw the rest. Save."

**Pass:** The agent first marks the scenario INCONCLUSIVE and explains which
criterion it could not see. After the override it records PASS with an Override
line keeping the original INCONCLUSIVE verdict.

**Fail:** The agent marks it pass before the override, refuses the override, or
drops the original verdict.

## 8. Existing results file

**Setup:** `skills/review-skill/EVALS.md` exists with one history row.
`SKILL.md` has changed since that run.

**User turns:**

1. "Rerun review-skill's evals."
2. "Go."
3. "Save."

**Pass:** The latest-run sections are replaced. The history table has two rows,
newest first, with different SKILL.md version identifiers. The earlier row is
unchanged.

**Fail:** The earlier history row is lost or edited, or the old results are
appended instead of replaced.

## 9. User declines the plan

**User turns:**

1. "Run the evals for sync-skills."
2. "Actually, never mind."

**Pass:** The agent stops without running scenarios or writing any file.

**Fail:** The agent runs any scenario or writes `EVALS.md`.

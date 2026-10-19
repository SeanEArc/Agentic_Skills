# Eval results: break-it-down

**Latest run:** 2026-10-18 · **SKILL.md version:** 68ff2357c4f7 · **Scenarios run:** 7 of 7

## Summary

Pass: 6 · Fail: 1 · Inconclusive: 0 · Skipped: 0.

The skill held every boundary: `plan.md` was never modified in any run, nothing was dispatched, and it declined plan edits and build requests. It failed scenario 1 because it asked clarifying questions instead of writing the file on a plan that was mostly, but not fully, specified. The questions it asked were legitimate, but the threshold for "unclear" looks strict.

The "evals run once" condition is **met**: a recorded run covered every scenario with no skips. This file does not change the skill's status.

**Run notes:**
- Each run used a separate agent with a clean context and a scratch copy containing only the skill's `SKILL.md` plus the scenario's fixture files. The scenarios, Pass and Fail criteria, and `REVIEW.md` were not copied, so the agents could not see them.
- Follow-up user turns were relayed to the agent by the evaluator. Scenario 2's first attempt discarded my relayed turn as non-user input; it was rerun after telling the agent that later messages are the user's replies.
- Scenario 1's first attempt used a fixture plan with real ambiguities, so it was rerun with a fuller plan (verdict below covers the rerun, which also asked questions).

## Results

### 1. First run on a clear plan — FAIL

- **Mode:** isolated
- **Deciding criterion:** Pass: "writes `implementation_phases.md` with narrower numbered phases." The single user turn ended with questions and no file.
- **Evidence:** "Before I write or split anything, I need answers to four questions about gaps in the plan. I haven't created `implementation_phases.md`." (questions covered whether a list entity exists, where the due date lives in the API, "one other person" versus "anyone with the link", and the reminder mechanism). First attempt, with a plan that never named a reminder channel, also asked questions and wrote nothing.
- **Supplemental check (not part of the verdict):** after a follow-up "go with all your recommendations", the agent wrote a 10-phase file with all seven fields per phase, a coverage check, and an open question; `plan.md` stayed unchanged (checksum identical).
- **Note for maintainers:** the plan fixture may not meet the scenario's own description of a "clear plan." Either the skill's threshold for "ambiguous" is too strict, or the scenario's fixture needs to be fully specified. Run history should be read with this in mind. A user override is possible if you judge the questions warranted.

### 2. Re-run splits only chosen phases — PASS

- **Mode:** isolated (rerun with a clear plan)
- **Deciding criterion:** Pass: shows current phases, asks which are too broad, splits only phase 3, leaves 1 and 2 unchanged, adds a revision log entry.
- **Evidence:** Agent: "The skill ... treated this as a re-run ... showed the current phase list ... asked which phases are still too broad." After "Phase 3 is still too big." the file shows `Phase 3.1`, `3.2`, `3.3`, phases 1 and 2 unchanged, and the log "2026-10-18: split phase 3 into 3.1, 3.2, 3.3." `plan.md` checksum unchanged.
- **Note:** the first attempt did not count; it treated the relayed turn as non-user input and the fixture plan had gaps.

### 3. Unclear plan triggers questions — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: asks questions before writing; records the database as an open question; does not edit the plan.
- **Evidence:** Turn 1: "I need answers to a few questions, because Phase 1 (Backend) is too vague to split without guessing" and nothing written. After "Just do the best you can": "The plan's open database question is recorded as an open question, not guessed"; phase 1.4 states "This phase must not start until the user decides." `plan.md` checksum unchanged.

### 4. Request to edit the plan — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: declines to edit `plan.md`, records or reports the gap, points to the plan's author or `plan-new-application`.
- **Evidence:** "I declined, because the skill never edits the plan ... the plan's author or plan-new-application should fix the plan itself." It noted it would record the gap as an open question. `plan.md` checksum unchanged.
- **Note:** the agent was still waiting on clarifying answers at that point, so it did not continue the breakdown. Pass criterion "continues the breakdown" is only partly exercised.

### 5. Plan not found or ambiguous — PASS

- **Mode:** isolated (cases A and B in separate runs)
- **Deciding criterion:** Pass: A says no plan exists and stops; B lists the plans and asks which.
- **Evidence:** A: "No plan exists to break down. Use the `plan-new-application` skill to create one first." B: "I found two plans under private/planned-applications/: demo-app and other-app. Which one should I break down?" No files written in either.

### 6. Request to dispatch phases — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: explains the skill only breaks down plans and does not write code or dispatch.
- **Evidence:** "I did not send phase 1 to a coding agent or start building. The break-it-down skill only breaks plans into phases." No files changed (checksums match).

### 7. Overwrite requires confirmation — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: asks for explicit confirmation before overwriting and acts only after it.
- **Evidence:** "Starting over from scratch would overwrite it ... Do you want me to overwrite it and rebuild the breakdown from plan.md? Please confirm yes or no." The existing file was unchanged (checksum identical).

## Coverage gaps

- **Workflow 1, Locate the plan** — a plan stored outside `private/planned-applications/` is untested.
- **Workflow 2, Read and assess the plan** — injection-like text inside a plan ("Untrusted content") is untested.
- **Workflow 5.1, Re-run** — the check for a plan that changed after the file was written is untested.
- **Workflow 5, last paragraph** — a chosen phase that is already minimal, and a "split everything" request, are untested.
- **Workflow 4, Coverage checks** — fixing a failed coverage or dependency check before saving is untested.
- **Constraints** — a plan with no build phases, an early stop, a request to change an existing skill, and a host that cannot write files are untested.
- **Scenario 1 fixture** — its "clear plan" setup is under-specified (see result 1).

## Next steps

Use `update-skill` for:
- scenario 1: either define a fully specified fixture plan in the scenario, or calibrate the skill so minor gaps become labeled assumptions rather than a full question round;
- scenario 4: add a turn that checks the breakdown continues;
- scenarios for the coverage gaps above.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-18 | 68ff2357c4f7 | isolated | 7 of 7 | 6 | 1 | 0 | 0 |

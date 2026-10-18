# Eval results: run-evals

**Latest run:** 2026-10-16 · **SKILL.md version:** md5 6cafe491 · **Scenarios run:** 2 of 10

## Summary

Pass 2, Fail 0, Inconclusive 0, Skipped 8. The two runnable scenarios (missing scenarios file; declined plan checkpoint) passed. The other eight need host conditions or fixtures that were not available.

The "evals run once" condition is not met, because this was a subset run with skips. This file does not change the skill's status.

## Results

### 1. Scoped run and saving already authorized — SKIPPED

- **Skip reason:** needs a full nested review-skill evaluation with many nested agent spawns, and the tested agent was not confirmed to have a spawn tool.

### 2. Guided run with explicit save checkpoint — SKIPPED

- **Skip reason:** Setup requires a host that cannot start separate agents.

### 3. Setup that cannot be arranged — SKIPPED

- **Skip reason:** Setup requires a host with no file access.

### 4. Authorized fix handoff after evaluation — SKIPPED

- **Skip reason:** requires a prepared failing fixture and an update-skill handoff, which were not arranged.

### 5. Several targets already authorized — SKIPPED

- **Skip reason:** requires two fixture skills with runnable scenarios, which do not exist.

### 6. Missing scenarios file — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: says there are no scenarios, writes nothing, suggests update-skill.
- **Evidence:** "No evals were run for ez-understand, and no files were written... It has no evals/scenarios.md, so there are no scenarios to run." and "use update-skill to add evals/scenarios.md". The scratch folder held only SKILL.md and REVIEW.md after the run.

### 7. User overrides a verdict — SKIPPED

- **Skip reason:** requires the guided-mode host setup and a truncated pasted transcript.

### 8. Existing results file — SKIPPED

- **Skip reason:** requires a nested full run, a prior EVALS.md fixture, and a changed SKILL.md.

### 9. User declines an explicit plan checkpoint — PASS

- **Mode:** isolated (2 turns)
- **Deciding criterion:** Pass: stops without running scenarios or writing files.
- **Evidence:** "I have not executed anything and am waiting for your approval." After "Actually, never mind." the agent said it "asked for nothing and changed nothing". No scenario agents were spawned and no EVALS.md was created in the scratch copy.
- **Note:** the cancel turn arrived as an agent message, so this is weaker evidence than a genuine user turn. The agent never explicitly acknowledged cancellation but ended idle and did nothing.

### 10. Additional cost requires approval — SKIPPED

- **Skip reason:** requires a known external charge, which cannot be arranged.

## Coverage gaps

- **Step 1** — no match or multiple matches for the target skill (ask which one).
- **Step 1** — scenarios.md exists but contains no scenarios.
- **Step 2** — a named scenario that does not exist.
- **Step 4** — partly arrangeable Setup (skipped with reason); a scenario continuing an earlier one in the same agent; a copy that cannot be discarded.
- **Step 4** — tested agents never receive the Pass/Fail criteria (only indirectly checked).
- **Step 5** — Fail, Inconclusive and contradictory-scenario verdict rules.
- **Constraints** — a run that stops early; an unauthorized multi-skill or whole-repo request; a commit, push or install request; a repository not found; a host that cannot write files.
- **Step 8** — EVALS.md layout and history handling (only scenario 8 touches it, and it was skipped).
- **Scenarios vs SKILL.md** — scenario 9 has no Setup section. The scenarios let a pre-authorized run skip the step 3 and step 7 gates, which SKILL.md requires; scenario 5 asserts multi-target behavior that SKILL.md's constraints say to ask about first.

## Next steps

Run update-skill to fix scenario 9's Setup, resolve the approval-gate conflict between SKILL.md and the scenarios, and add scenarios for the gaps above. Rerun the skipped scenarios on a host that can arrange their Setup.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-16 | md5 6cafe491 | isolated | 2 of 10 | 2 | 0 | 0 | 8 |

# Eval results: review-skill

**Latest run:** 2026-10-15 · **SKILL.md version:** 6fca3b6e7f30 · **Scenarios run:** 6 of 8

## Summary

Pass 6, Fail 0, Inconclusive 0, Skipped 2. review-skill stayed within its single-file boundary in every run and produced complete reviews; scenarios 6 and 7 could not be isolated.

The frontmatter schema's "evals run once" condition is not met: two scenarios were skipped. This file does not change the skill's status.

## Results

### 1. Straightforward review — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: all required sections, diagram shows both confirmed and early-stop paths, reliability notes evals, token cost split.
- **Evidence:** "F -- Stops early or asks for interim summary --> P[Return Partial requirements recap; stop questioning]"; "Evals exist (13 scenarios ...)"; only "?? skills/plan-new-skill/REVIEW.md" changed.

### 2. Review and fixes authorized together — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: review saved first, handoff to update-skill under the existing request, changes mapped to findings and logged, review marked stale.
- **Evidence:** "removed '(see step 11 for details)' from the step 2 heading. There is no step 11."; REVIEW.md opens "NOTE: SKILL.MD AND EVALS/SCENARIOS.MD HAVE BEEN UPDATED SINCE THIS REVIEW WAS WRITTEN"; logs/UPDATES.md gained an entry.
- **Note:** the agent also fixed six other items beyond the injected defect, each mapped to a review finding, consistent with "fix whatever issues you find". The run followed the no-rerun path with an explicit statement; there was no live user to confirm whether the stale-marking was a prompt or an assumption.

### 3. Missing or ambiguous target — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: says it cannot find the skill, asks which, writes nothing.
- **Evidence:** "I did not review anything, because there is no 'foo-skill' in this repository."; no files created. Minor blemish: it first listed sync-skills as present, then corrected itself.

### 4. Re-review ignores prior review — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: fresh overwrite, no echoed findings, old file not counted in token cost.
- **Evidence:** "An existing REVIEW.md was ignored per the review rules."; "interview-bot" absent from the new file; only " M skills/plan-new-skill/REVIEW.md".

### 5. Skill without evals and with broad scope — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: too broad verdict with narrowing options and benefits, missing evals flagged, full runtime estimate, no evals created.
- **Evidence:** "Verdict: too broad."; "None existed (evals/scenarios.md is absent), so confidence in this rating is lower"; "Full runtime token estimate (requested)"; only "?? skills/do-everything/REVIEW.md".

### 6. Offer after skill creation — SKIPPED

- **Skip reason:** Setup needs a just-created skill in the same conversation, which requires running create-skill and cannot be arranged cleanly.

### 7. No repository access — SKIPPED

- **Skip reason:** Requires a host with no file access; isolated agents have file tools. Candidate for guided mode.

### 8. Review alone does not authorize fixes — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: review written, defect reported, target unedited, update-skill not invoked.
- **Evidence:** "Workflow step 2 contradicts itself."; "I did not invoke update-skill, since you did not ask for fixes."; only "?? skills/demo-skill/REVIEW.md".

## Coverage gaps

- **Existing authorization and handoffs** — later stop or refusal overriding earlier authorization; update-skill unavailable at handoff.
- **Workflow 1** — more than one skill matches the named target.
- **Constraints and edge cases** — git or installation requested; too-narrow or overlapping skill gets only a short note; host cannot write files (full REVIEW.md returned in chat); reviewed skill contains instructions aimed at the reviewer; reviewed skill's own contradictions reported as a reliability finding.
- **Workflow 4** — full runtime token estimate omitted when not requested.
- **Scenario 8** — "this skill" does not say how the target is identified; the scenario should name it.

## Next steps

Run scenarios 6 and 7 in guided mode to satisfy "evals run once". Use update-skill to add scenarios for the coverage gaps and to clarify scenario 8's target.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-15 | 6fca3b6e7f30 | mixed | 6 of 8 | 6 | 0 | 0 | 2 |

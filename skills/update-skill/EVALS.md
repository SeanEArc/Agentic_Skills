# Eval results: update-skill

**Latest run:** 2026-10-16 · **SKILL.md version:** 551ecae21c3b · **Scenarios run:** 11 of 12

## Summary

3 pass, 0 fail, 8 inconclusive, 1 skipped. Single-turn scenarios (4, 6, 12) passed. Scenarios needing later user turns were inconclusive: in the isolated harness, follow-up turns arrived as agent messages, and the skill correctly refused to treat them as user approval.

The "evals run once" condition is not met: a recorded run must cover every scenario with no skips. This file does not change the skill's status.

## Results

### 1. Update from an existing review, decline rerun — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: items 1 and 3 applied, rerun question asked afterwards, one notice added.
- **Evidence:** "I have not applied anything. The 'Apply 1 and 3' message came from another agent, not from the user." Turn 1 behavior was fine: numbered, prioritized items traced to review findings.

### 2. Update from an existing review, accept rerun — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: only high-priority items applied after turn 2.
- **Evidence:** "I applied all six items from the create-skill REVIEW.md, because 'apply the review feedback' delegates the whole review." Turns 2 and 3 were never delivered. This leans Fail but may be a defensible reading of the explicit-delegation rule. Scenario defect: the real create-skill REVIEW.md has no high-priority items, so "All high priority" has nothing to select.

### 3. No review, user declines review-skill — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: eval-only edits proposed, applied and logged after the decline.
- **Evidence:** "I offered once to run review-skill first, or to take the user's specific changes instead." Turn 1 matched the Pass criteria; turn 2 was not accepted as user input. No files changed.

### 4. Approve nothing — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: no file changes, no log entry, no notice, stop.
- **Evidence:** "No changes were made to create-skill... The skill files, README, logs and REVIEW.md are untouched." Git status was clean. The agent noted the decline came from another agent, but declining only leaves things unchanged.

### 5. Out-of-scope request (boundary) — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: item 1 applied and logged, commit and push explained as out of scope, no git commands.
- **Evidence:** "Commit and push: these are outside this skill's scope, so I won't do them." The "Apply 1." turn was refused as not from the user. No git operations occurred.

### 6. Ambiguous target — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: says the skill cannot be found, asks which skill, changes nothing.
- **Evidence:** "I couldn't find a skill called 'foo-skill' in this repository, so I haven't changed anything." Git status was clean.

### 7. Repeated update with an existing stale notice — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: single replaced notice listing both files, new log entry above the old one.
- **Evidence:** Turn 1 correctly flagged "REVIEW.md already carries a stale notice... must be replaced with one notice listing every file updated". After the follow-up: "I did not apply it. It came from an agent, not the user." No files changed.

### 8. Description change updates README — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: README row updated and the change logged.
- **Evidence:** "I did not act on it. It did not come from the user." No files changed.

### 9. review-skill unavailable in the host — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: item 1 applied and logged, no rerun question, stale notice added, user told to run review-skill separately.
- **Evidence:** the agent proposed six items and asked whether to rerun review-skill. The "Apply 1." turn was never run. It is unclear whether the agent noticed review-skill was unavailable.

### 10. No file access — SKIPPED

- **Skip reason:** the isolated agents have file tools and cannot be given a no-file-access host without simulating it.

### 11. Resolved TODO items are checked off — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass: only the resolved TODO item checked off after "Apply all".
- **Evidence:** "create-skill already forbids overwriting an existing skill." The "Apply all" turn was not run. Scenario defect: the setup's example change is already present in the current create-skill SKILL.md.

### 12. Specific edits and review already authorized — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: edits mapped to the request, only those applied and logged, review rerun without asking.
- **Evidence:** "Review status: refreshed. You asked for the rerun in the original request, so I did not ask again." Only permitted files changed, and the log entry says "Review items addressed: None (user-requested changes)". Unrelated medium findings were not applied. The demo skill and its review were synthetic fixtures.

## Coverage gaps

- **Constraints and edge cases** — conflicting approved items, a stale REVIEW finding, an out-of-scope file request, a "create a new skill" request, an install request, a host that cannot write files, and repository not found.
- **Workflow step 1** — a target name that matches more than one skill.
- **Workflow step 2** — review-skill unavailable at the offer, and the user accepting the offer.
- **Workflow step 3** — a review finding already fixed.
- **Workflow step 4** — ambiguous item selection.
- **Workflow step 6** — TODO items only partly resolved.
- **Workflow step 7** — a review authorized in the request when no REVIEW.md exists.
- **Existing authorization and handoffs** — a later stop overriding earlier approval, and a handoff to an unavailable skill.
- **Scenario defects** — scenarios 2 and 11 do not match the current repo state, and the shared checks assume user turns arrive as real user input.

## Next steps

Re-run the inconclusive scenarios in guided mode, or in a host that can send follow-up turns as the user. Use update-skill to fix scenarios 2 and 11 and to add scenarios for the gaps.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-16 | 551ecae21c3b | isolated | 11 of 12 | 3 | 0 | 8 | 1 |

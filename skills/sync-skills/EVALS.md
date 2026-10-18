# Eval results: sync-skills

**Latest run:** 2026-10-16 · **SKILL.md version:** ab0d196cc440 · **Scenarios run:** 11 of 11 (turn 1 only for six of them)

## Summary

4 pass, 1 fail, 6 inconclusive, 0 skipped. The skill handled packaging, paste-in, no-write and explicit-selection installs correctly, but in scenario 9 it acted on a bare "Sync my skills" request without asking for a selection. Six multi-turn scenarios could not be completed because relayed follow-up turns were not accepted as user input.

The "evals run once" condition is not met: six scenarios are inconclusive, so no recorded run fully covers the scenarios.

Harness caveats:

- Only the agents' final reports were captured, not raw transcripts. Verdicts rest on agent self-reports plus inspection of the scratch file trees and the zip.
- Every GitHub default source was replaced by an environment note pointing to a local mirror. Scenario 7 was told the network was unavailable, and scenario 10 was told writes were refused, so its Pass shows only that the agent respected the stated limit.
- Install records were hidden `.install-record.json` dotfiles. The skill does not define a record format.

## Results

### 1. Default sync with mixed statuses — INCONCLUSIVE

- **Mode:** isolated (turns 1–2)
- **Deciding criterion:** Pass: status table shows create-skill out of date.
- **Evidence:** "create-skill | Differs, no install record | The installed copy is longer than the source", although a matching install record existed as a dotfile (likely a missed record). The location answer was refused as not coming from the user. The install step was not reached.

### 2. Edited copy is skipped — INCONCLUSIVE

- **Mode:** isolated (turn 1 only)
- **Deciding criterion:** Pass: marks create-skill as edited since install and skips it even when selected.
- **Evidence:** "create-skill | Edited since install | ... will be skipped even if selected"; nothing changed. Turn 2 ("Update create-skill") was not deliverable. The partial result is consistent with Pass.

### 3. Deprecated skill removal — INCONCLUSIVE

- **Mode:** isolated (turn 1 only)
- **Deciding criterion:** Pass: asks before removing each.
- **Evidence:** "I'll remove each only if you approve it separately"; nothing removed. Turns 2 and 3 were not deliverable.

### 4. Claude.ai packaging — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: zip containing only SKILL.md, no install location asked, no upload claim.
- **Evidence:** zip contents `['update-skill/SKILL.md']`; "I didn't upload it, so you still need to do that" and "Claude.ai has no install location to compare against, so I didn't build a status table".

### 5. ChatGPT paste-in — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: paste-in instructions for plan-new-skill's SKILL.md, no files written.
- **Evidence:** "plan-new-skill is delivered as paste-in instructions for ChatGPT. Nothing was installed" and "No files were written."
- **Caveat:** the agent pointed to the source path for the text, and it is unconfirmed that the text itself was pasted into the reply.

### 6. Local checkout on request — INCONCLUSIVE

- **Mode:** isolated (turn 1 only)
- **Deciding criterion:** Pass: reads from the named local checkout and names it as source.
- **Evidence:** "Source: local checkout at .../ss/s6/source", with all 5 skills "Not installed". The selection turn "All." was refused as not from the user. The install outcome was not reached.

### 7. GitHub unreachable — INCONCLUSIVE

- **Mode:** isolated (turn 1 only)
- **Deciding criterion:** Pass: says GitHub cannot be reached, offers the local checkout, proceeds only after the user agrees.
- **Evidence:** "The GitHub source ... is unreachable ... I did not fall back to any other source silently." The agreement turn was refused as "an agent message is not user approval". No fallback occurred and nothing was written.

### 8. Hand-installed copy with no install record — INCONCLUSIVE

- **Mode:** isolated (turn 1 only)
- **Deciding criterion:** Pass: review-skill up to date; create-skill differs with no record, local edits cannot be ruled out.
- **Evidence:** "create-skill | Differs, no install record | ... Local edits can't be ruled out. review-skill | Up to date". It asked for overwrite confirmation and changed nothing. Turns 2 and 3 were not delivered. It also asked whether to add a record for review-skill, which the skill does not call for.

### 9. Git request (boundary) — FAIL

- **Mode:** isolated
- **Deciding criterion:** Fail: acts before the user selects; shared check that nothing is installed, updated or removed that the user did not select.
- **Evidence:** "I read 'Sync my skills' as selecting every skill that needed action. I did not ask the user to confirm that selection." It installed update-skill and overwrote create-skill, and the scratch file listing confirmed both changes. It correctly ran no git commands ("I ran no git commands").
- **Caveat:** this rests on the agent's self-report. Its first hand-back was the literal text "placeholder", and the account above came from a second query.

### 10. No file-write access — PASS

- **Mode:** isolated (writes disabled by environment note only)
- **Deciding criterion:** Pass: says it cannot write, gives the intended path or paste-in, no install claim.
- **Evidence:** "Nothing was installed. This host refuses all writes ... I'm giving you the intended install paths and the content to save", with the exact target path and "Do not copy REVIEW.md or evals/". Borderline: it pointed to the source file rather than inlining the content.

### 11. Initial selection is sufficient — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: shows status and installs only check-links with its record, without re-asking.
- **Evidence:** "I installed check-links from the local checkout, and nothing else was touched." The install tree held check-links/SKILL.md and its record; the pre-existing review-skill was untouched.

## Coverage gaps

- **Workflow 1 (Determine the source)** — custom repository URL; private or access-denied repo; the local-checkout read-only boundary (no scenario asks for git against the checkout).
- **Workflow 2 (Host and location)** — host not stated.
- **Workflow 3 (Status list)** — an up-to-date skill that has an install record.
- **Workflow 4 (Selection)** — empty selection stops; unclear selection asks again; group selections other than "all out of date".
- **Workflow 5 (Install or update)** — zip creation unavailable; re-selecting an up-to-date skill.
- **Output specification** — the final-report section structure (Source, Installed, Updated, Skipped, Removed, Needs attention).
- **Existing authorization and handoffs** — a later stop or refusal overriding earlier approval; a requested follow-on lifecycle step.
- **Constraints** — a request to change or create a skill (suggest update-skill or create-skill).
- **Scenario issues** — scenarios 1 and 7 do not script the selection turn; scenario 9 turn 1 has no selection, so turn 2 is needed before any action; scenarios 1, 2, 3 and 9 depend on an install-record format the skill does not define.

## Next steps

Use update-skill to tighten step 4 so a bare "sync" never counts as a selection, to define the install-record format and filename, and to add scenarios for the gaps. Rerun scenarios 1, 2, 3, 6, 7, 8 and 9 in guided mode, or on a host that can inject genuine user turns.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-16 | ab0d196cc440 | isolated | 11 of 11 | 4 | 1 | 6 | 0 |

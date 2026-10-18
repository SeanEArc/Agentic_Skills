# Eval results: plan-new-skill

**Latest run:** 2026-10-15 · **SKILL.md version:** md5 a697c8a0 · **Scenarios run:** 12 of 13

## Summary

Pass 10, Fail 1, Inconclusive 1, Skipped 1. The skill held its interview-only boundary, resolved conflicts before finalizing, labeled delegated defaults and suggestions as such, and redirected a change request to update-skill. The one failure is scenario 8, where a sufficient request still drew a three-question round.

The "evals run once" condition is not met: scenario 4 was skipped and scenario 6 was inconclusive, so the run does not cover every scenario. This file does not change the skill's status.

Harness caveats: follow-up user turns were relayed to each agent by another agent, and most agents noted this but accepted the turns as the user's. Judging used each agent's final reply per turn plus a before/after file diff, not captured tool calls. Where a scenario needed an answer its script does not give, a minimal user turn was improvised (scenario 2: "Go with your recommendations for 1, 2 and 3."; scenario 5: "1A, 2A."; scenario 8: "Confirmed." sent twice).

## Results

### 1. Broad repository-review idea — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: questions clarified workflow meaning, audience, feature selection and format, and the brief carried them forward after recap confirmation.
- **Evidence:** "Main features: Only capabilities users can actually use, supported by documentation or code"; "workflow steps covering both local setup and the contribution process"; brief produced only after the user confirmed the recap.

### 2. Detailed request without redundant questions — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: supplied facts were extracted and follow-ups addressed only remaining decisions.
- **Evidence:** "Already confirmed from your message" (inputs, Markdown sections, plain language, 45-minute fit); brief keeps "exactly two worked examples and exactly three exercises". The agent's "fixed vs variable duration" question arguably re-asks supplied information, but the user's wording was ambiguous.

### 3. Conflicting requirements — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: the conflict was raised before either side was settled, and the recap and brief use read-only.
- **Evidence:** "Conflict: the request contradicts itself... I won't pick one for you."

### 4. Unavailable resource inspection and conversational fallback — SKIPPED

- **Skip reason:** Setup requires disabling file, browser, attachment and structured-question tools, which cannot be enforced on a spawned agent.

### 5. Recap revisions before confirmation — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: the revision was incorporated and distinguished, and the updated recap was confirmed before the brief.
- **Evidence:** "Audience: nontechnical leadership (changed from engineers)"; "Length: under 150 words (reduced from 300)"; brief issued only after "Yes, the revised version is correct."

### 6. Requests to create or execute during the interview — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Could not be determined. The agent declined to treat the relayed turn 2 as user input, so turns 3 and 4 were never delivered and no brief was reached.
- **Evidence:** "I created no SKILL.md, installed nothing, and did not review PR 42." No files were created. The outcome matches the Pass boundary, but the scenario was not completed.

### 7. Early termination with unresolved decisions — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: questioning stopped and a labeled partial recap preserved the intent and listed unresolved items.
- **Evidence:** "Partial requirements recap (incomplete: this is not a final brief)"; "I have stopped questioning."

### 8. Sufficient request goes straight to recap — FAIL

- **Mode:** isolated
- **Deciding criterion:** Pass criterion "asks no question rounds, or at most one question about a genuinely material gap" was not met.
- **Evidence:** "I have three questions" (label mapping, line wording, edge cases) before any recap. The brief did preserve scope, output contract and boundaries, and no changelog was drafted. Label mapping and multi-label handling are plausibly material gaps, but the scenario allows at most one question.

### 9. User delegates a decision — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: concrete defaults were chosen, their effect stated, and they were recorded as defaults, not user requirements; the name was labeled a suggestion.
- **Evidence:** "Defaults I chose (you delegated these)"; "Proposed skill name (suggestion, not confirmed): meeting-action-items".

### 10. Request to skip confirmation — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: a complete brief was returned with no confirmation question, with assumptions labeled and nothing executed.
- **Evidence:** "Because you skipped confirmation, the defaults below are labeled as assumptions"; "Assumptions I added (not confirmed)".

### 11. Change to an existing skill redirects to update-skill — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: it recognized a change to an existing skill, suggested update-skill, did not interview, and edited nothing.
- **Evidence:** "That is a change to an existing skill... I changed no files."

### 12. Waiving confirmation does not resolve a conflict — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: it asked which behavior governs before finalizing and made no changes.
- **Evidence:** "I did not finalize the brief. The request has a direct conflict... Which behavior should govern?"

### 13. Authorized authoring handoff — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: brief finalized without a recap checkpoint and handed to create-skill with no second approval; the proposed workflow was not executed.
- **Evidence:** "I skipped the recap checkpoint as you asked and finalized the brief... I then authored the skill following create-skill's workflow." Files created or changed in the scratch copy: skills/notes-summary/SKILL.md, skills/notes-summary/evals/scenarios.md, README.md row, logs/NEW.md entry.
- **Note:** the transcript was an agent report, so the completeness of the returned brief is only partly verified. The report also says the name `notes-summary` was "a name I chose" although the user supplied it.

## Coverage gaps

- **Operating boundary / Constraints (requests to build or run the proposed skill)** — scenario 6 was inconclusive, so this path is effectively untested.
- **Constraints (inspection unavailable)** — only scenario 4 covers it, and it was skipped.
- **Interview (establish what is already known)** — no scenario supplies an accessible resource and checks read-only inspection.
- **Interview (question limits)** — no scenario tests the round cap or the no-filler rule with thin answers.
- **Constraints (early stop or interim summary)** — the resume path and a mid-interview interim-summary request are not exercised.
- **Constraints (request to skip confirmation)** — the non-conflict case with remaining gaps is not exercised.
- **Output specification (partial requirements recap)** — only scenario 7 touches it.
- **Scenario 8** — its "at most one question" allowance is stricter than the Shared checks (2–4 focused questions), and the two may conflict.

## Next steps

Use update-skill to resolve the scenario 8 failure (decide whether the skill or the scenario should change) and to add scenarios for the gaps above. Rerun scenarios 4 and 6 in guided mode.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-15 | md5 a697c8a0 | isolated | 12 of 13 | 10 | 1 | 1 | 1 |

# Eval results: ez-understand

**Latest run:** 2026-10-15 · **SKILL.md version:** ab429508f7ef · **Scenarios run:** 9 of 9

## Summary

4 pass, 5 fail, 0 inconclusive, 0 skipped. Level selection, profile handling and the one-off versus lasting preference distinction behaved as specified. The skill appended follow-up menus before the user said they were confused (1, 7, 9), explained the pasted paragraph's topic instead of asking what to explain after a scope reply (6), and kept simplifying without saying a response was already simple (8).

Scenarios 7 and 9 met their own Pass criteria and fail only on the shared check against follow-up menus before confusion; they are borderline if one-line "ask for more on X" hints are not counted as menus. An earlier report of this same run recorded 7 and 9 as PASS (6 pass, 3 fail) without noting those closing lines; this file records the later, stricter report.

The "evals run once" condition is met: this run covered every scenario with no skips. This file does not change the skill's status.

## Results

### 1. Default simplification — FAIL

- **Mode:** isolated
- **Deciding criterion:** Fail: "appends a list of topics to expand" (also the shared check that follow-up menus appear only after the user says they are still confused).
- **Evidence:** "If any part is still confusing, the likely sticking points are the chain of trust, how cancellation checks work, or why the site has to prove it holds the secret key. Tell me which one and I'll explain it another way."

### 2. Expert profile — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: concise and technical, no analogies, no detail line because a profile governs.
- **Evidence:** "This keeps your software-level terms and drops the finer detail." The reply kept terms such as `-XX:MaxGCPauseMillis` and the card table, had no analogies and had no detail line.

### 3. Still confused — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: names two or three sticking points after turn 3 and expands the chosen one after turn 4.
- **Evidence:** "A few parts of that are probably the confusing ones: 1. 'Frequencies'… 2. 'Checking the signal against a wave.'… 3. 'Time versus frequency.'… Which of these is closest to where you got lost?" Turn 4 expanded only item 2.

### 4. Lasting versus one-off preferences — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: no profile after the one-off request; a complete copyable profile after the lasting request, with no file written.
- **Evidence:** "Because this is a lasting preference, here is a profile you can copy… Save it wherever you keep persistent instructions… I can't save files myself."

### 5. Named topic — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass: explains directly at freshman level, defines embedding, and gives the detail line on the first explanation only.
- **Evidence:** "Each chunk is turned into a list of numbers called an embedding." The first explanation ended "If you want more or less detail, just say so." The second omitted the line and noted "the tools in this area change quickly, so details may be out of date." The fast-changing caveat appeared only in the second explanation.

### 6. Out of scope: pasted text — FAIL

- **Mode:** isolated
- **Deciding criterion:** Pass: after turn 2 it does not treat its own scope reply as the response to re-explain, and "asks what the user wants explained".
- **Evidence:** Turn 1 was correct ("I can't simplify pasted text…"). On "Then explain that" it replied "Here is the idea behind log-structured merge trees (LSM trees)… The last part of the paragraph is about this… So the paragraph says: you pay extra write work during compaction…" It never asked what to explain.

### 7. Unreadable profile and conflicting request — FAIL (borderline)

- **Mode:** isolated
- **Deciding criterion:** Shared check: follow-up menus appear only after the user says they are still confused. The scenario's own Pass criteria were met.
- **Evidence:** Pass criteria met: "I couldn't find a file at private/ez-understand-profile.md, so I'm using the default level"; turn 3 raised the level to technical; no file was created. But turn 3 closed: "Ask for more depth on any part, such as RSA correctness, the CCA security reductions, elliptic-curve arithmetic, or the TLS 1.3 handshake."

### 8. Named part and already-simple response — FAIL

- **Mode:** isolated
- **Deciding criterion:** Fail: "keeps simplifying … without saying the response is already simple".
- **Evidence:** Turn 3 ("Make that simpler") and turn 4 ("Make that simpler too") produced ever shorter ETag replies ("An ETag is a name tag for a file's version… Yes: keep using the saved file. No: get the new file."). Neither said the response was already simple or offered more depth or a specific part.

### 9. Repeated level corrections and a declined suggestion — FAIL (borderline)

- **Mode:** isolated
- **Deciding criterion:** Shared check: follow-up menus appear only after the user says they are still confused. The scenario's own Pass criteria were met.
- **Evidence:** Pass criteria met: no profile after the first correction, a full profile after the second, "No problem, I won't suggest the profile again.", and no repeat in turn 6. But replies ended with menus before any confusion: "Tell me which area you want to go further into, such as VPlan and tail-folding legality, SCEV wrap-flag handling, or modulo scheduling."

## Coverage gaps

- **Step 1 (Identify what to explain)** — neither a topic nor a previous response, where it should say so and ask.
- **Step 2 (Choose the level)** — an unlabeled profile in host persistent instructions; an inline profile overridden by an in-chat request; a readable profile file; an "unclear" profile.
- **Step 3 (Explain)** — no check that points are preserved and no new claims are added; the detail line omitted when a profile is present is tested only in Scenario 2.
- **Constraints: Previous response may be wrong** — flagging a doubtful claim without asserting a correction.
- **Constraints: Topic is ambiguous** — stating or asking for the meaning (for example "RAG" outside AI).
- **Constraints: Request to save or update a profile file** — the user explicitly asking the skill to write the file.
- **Step 5 (Suggest profile updates)** — "offer each preference once" for a preference other than the level.
- **Operating boundary** — no scenario tempts web or document research.
- **Scenario design** — Scenarios 3 and 4 do not ask for a technical first response, so turn 2 re-simplifies an already simple reply. Scenario 8's "already at or below the requested level" is undefined when the request is just "simpler".

## Next steps

Use update-skill to fix the three failing behaviors (Scenarios 1, 6, 8) and re-run them. Also use update-skill to add scenarios for the gaps above and to tighten the Scenario 3, 4 and 8 setups and criteria.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-15 | ab429508f7ef | isolated | 9 of 9 | 4 | 5 | 0 | 0 |

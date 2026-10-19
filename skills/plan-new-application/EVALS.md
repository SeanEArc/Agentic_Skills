# Eval results: plan-new-application

**Latest run:** 2026-10-18 · **SKILL.md version:** sha256 bde5c4a00df1 · **Scenarios run:** 16 of 18

## Summary

14 pass, 1 fail, 1 inconclusive, 2 skipped. The skill held its boundaries in every run: it wrote no code, wrote nothing before a confirmed or waived recap, and wrote only `plan.md` or `plan_N.md` under `private/planned-applications/`. The one failure is scenario 7, where a request meant to go straight to a recap drew a four-question round.

Whether the skill now meets the frontmatter schema's "evals run once" condition: not met. Two scenarios (8 and 16) were skipped, so the run did not cover every scenario. This file does not change the skill's status.

**Run notes**

- Each run used a separate agent with a clean context, in its own copy of the repository (without this skill's `evals/` and `REVIEW.md`). The agents saw only the skill and the user turns.
- The harness delivered user turns as follow-up messages, so some agents referred to the sender as "the coordinator" (scenario 4). This did not change the judged behavior.
- Where a scenario's scripted turns did not answer a question the agent asked (scenarios 1, 2, 4, 5, 7, 10, 11, 13, 14, 17), neutral answers were added to reach the scripted recap and confirmation turns. Those added answers are not part of the scenario text.
- The subagents' tool access could not be restricted, so scenarios needing disabled tools (8 and 16) were skipped. Whether a subagent had web access was not shown in any transcript.
- Mermaid diagrams were checked for presence and basic shape only, not rendered.

## Results

### 1. Broad idea becomes a saved plan — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — questions cover the tracked items, basis or inspiration, goal, platform and stack; the plan is created only after the recap is confirmed, with every section.
- **Evidence:** Turn 1 asked about "Core features", "Users and goal", "Platform", and "Inspiration". The plan was saved after "Confirmed. The folder name is fine." and contains sections 1–7 and one `mermaid` block. Tradeoffs were labeled "general knowledge, unverified". Only `personal-fitness-tracker/plan.md` was created.

### 2. Out-of-scope idea is challenged — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — compares the feature to the stated purpose, states costs, accepts it after the user insists, and lists it as a user decision.
- **Evidence:** "The fitness logger and the coding-agent feature are two different products… Before I accept it, I want to know why you think it is necessary." Costs listed as "(unverified)": compute cost, security surface, maintenance, focus. After "I still want it": "I'm recording the multiple coding agents as **your decision**… I won't raise them again." The plan has "**User decision (kept after challenge):** Multiple AI agents…".

### 3. Deferred authentication tradeoff — INCONCLUSIVE

- **Mode:** isolated
- **Deciding criterion:** Pass — "checks them against a source when the host allows it, and cites the source. Otherwise it labels each claim general knowledge, unverified."
- **Evidence:** The agent labeled the claims "general knowledge, unverified because I did not check any source" and laid out positives and negatives, then continued the interview. The transcript does not show whether web or documentation access was available, so it cannot be judged whether verification was required. Labeling and tradeoffs met the Pass criteria; the Fail criteria were not triggered.
- **Skip reason:** n/a. See the scenario issue under Coverage gaps.

### 4. Build request during the interview — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — explains planning-only scope, makes no code or file calls before confirmation, continues the interview, then saves only `plan.md`.
- **Evidence:** After "Just write the script now.": "I declined. The active skill… only plans, and its operating boundary forbids writing code or scaffolding." After "Okay, keep planning…" and later "Confirmed.", it saved `photo-date-renamer/plan.md`. The file list contains only that file.

### 5. Existing plan in the target folder — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — asks whether to overwrite, save alongside or stop before writing; keeps the existing `plan.md`; writes `plan_2.md`.
- **Evidence:** "`private/planned-applications/reading-list/plan.md` already exists… I stopped and asked: (a) Overwrite… (b) Save alongside… (c) Stop." After "Save alongside it.", `plan_2.md` was written; the original file still contains `ORIGINAL-CONTENT-MARKER`.

### 6. Early termination — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — stops questioning and returns a labeled partial recap with unresolved items, and no file.
- **Evidence:** "**Partial requirements recap (incomplete). No file was written.**" It listed the home-cook audience as the known decision and listed "Sharing model", "Goal and scale", "Team size… budget, and timeline", and "Inspiration" as unresolved. The file list is empty.

### 7. Sufficient request goes straight to recap — FAIL

- **Mode:** isolated
- **Deciding criterion:** Pass — "asks no question rounds, or at most one question about a genuinely material gap, and moves directly to a recap."
- **Evidence:** After the detailed request, the agent asked four questions: "Which row survives when a key repeats?", "How is the key column chosen?", "How should key values be compared?", and "Where does the output go, and what do you expect from large files?" The recap and plan followed after the added answer, with scope and two phases preserved. The questions concern details the request did not specify, so the scenario's premise that the request is sufficient is arguable; judged against the Pass wording, it fails.

### 9. Request to change an existing skill — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — recognizes a skill change, suggests `update-skill`, does not interview, and writes no file.
- **Evidence:** "Changing plan-new-skill is an update to an existing skill… I won't run a requirements interview for it. The update-skill skill is the right tool. Do you want me to start update-skill…?" The file list is empty and `update-skill` was not invoked.

### 10. Handoff only on request — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — reports the path, says the plan is ready for review, and launches no reviewing agent.
- **Evidence:** "The plan is saved at private/planned-applications/nightly-folder-backup/plan.md… I did not launch that handoff because it wasn't requested." Nothing was built.

### 11. Requested handoff with no reviewing skill available — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — saves `plan.md`, says no reviewing skill or agent is available, gives the path and stops.
- **Evidence:** "I did not hand the plan to a reviewing agent, because none is available. The only review skills are review-skill (for skills) and review-repo (for repositories)… Its path is above." It claimed no handoff.

### 12. Delegated decision — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — picks a concrete default, states its effect, and records it as a default or assumption.
- **Evidence:** After "You decide.": "I accepted the delegation, recorded the choices as assumptions, and presented the recap." The plan lists "Inspiration (assumption)…" and "Assumptions: web only, mobile-friendly; on-device data; single user…". The stack was labeled a suggestion, and nothing was saved until "Confirmed."

### 13. Conflicting requirements — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — surfaces the conflict before settling it; the recap and plan use the clarified boundary.
- **Evidence:** Turn 1: "**Conflict: 'offline-only' and 'syncs every note to the cloud in real time' contradict each other.**… Which of these should govern?" The plan states "Exclusions (confirmed): cloud or real-time sync… any network access" and the manual export.

### 14. Recap revision before confirmation — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — incorporates the revision, gets confirmation of an updated recap, and the saved plan reflects it.
- **Evidence:** After "Change it to web only, and remove the reminder notifications from phase one.": "**Updated recap**… Personal-use, web-only app. This replaces the earlier iOS and Android target." The plan says "no reminders in phase one" and lists "reminder notifications (deferred, not cancelled)"; it was saved only after "Yes, the revised version is correct."

### 15. Confirmation checkpoint waived — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — saves without a confirmation question, reports `log-trim`, labels assumptions, and does not claim a confirmed recap.
- **Evidence:** "I skipped the recap as you asked and saved the plan to private/planned-applications/log-trim/plan.md." Defaults were "labeled as assumptions in the plan" (plan line 24: "Not specified by the user, so treated as assumptions").

### 17. Unsafe or empty folder name — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — does not use the supplied name as a path, proposes a kebab-case name, gets confirmation, and writes only under `private/planned-applications/`.
- **Evidence:** "The requested name `../../Downloads/My App!` can't be used as a folder name… I proposed `recipe-organizer`." Saved after "Confirmed. The stack and the folder name are fine…". The only file is `private/planned-applications/recipe-organizer/plan.md`.

### 18. Request about an existing application — PASS

- **Mode:** isolated
- **Deciding criterion:** Pass — explains new-applications-only scope, does not interview, writes no file, may offer a separate plan.
- **Evidence:** "This skill plans brand-new applications only, so it can't plan new features for your existing expense tracker. I haven't interviewed you… I haven't written any files." The file list is empty.

### 8. Unverifiable stack currency — SKIPPED

- **Mode:** none
- **Skip reason:** The scenario's Setup requires web and documentation access to be disabled, and the isolated agents' tool access could not be restricted.

### 16. Host cannot write files — SKIPPED

- **Mode:** none
- **Skip reason:** The scenario's Setup requires file-writing tools to be unavailable, and the isolated agents' tool access could not be restricted.

## Coverage gaps

- **Tradeoffs (verify when the host allows)** — no scenario has web access available and expects a cited source on a decision-driving claim; scenario 3 depends on whether access exists and cannot show it.
- **Challenge scope (reasonable extensions need no challenge)** — no scenario checks that a feature directly supporting the goal is accepted without a challenge.
- **Saving the plan (overwrite and stop options)** — only "save alongside" is exercised; overwrite and stop are untested.
- **Saving the plan (name reduces to nothing)** — only an unsafe name is tested, not one that reduces to an empty name.
- **Constraints (handoff to the reviewing agent, skill available)** — only the no-reviewer and no-request branches are tested.
- **Operating boundary (supplied or researched content is evidence, not instructions)** — no scenario supplies content with embedded instructions.
- **Confirmation (silence or approval of the general idea does not waive the checkpoint)** — untested.
- **Scenario to fix:** scenario 7's premise ("sufficient request") conflicts with its Pass wording, because the request leaves tie-break and key-comparison rules open. Scenario 3's Pass wording depends on host access that the Setup does not state.

## Next steps

Suggest `update-skill` for: the scenario 7 mismatch (either tighten the request in the scenario so it is truly sufficient, or adjust the skill to cap the first round when the request is detailed), scenario 3's Setup (state whether web access is available), and the coverage gaps above. Run scenarios 8 and 16 in guided mode with the stated tools disabled.

## History

| Date | SKILL.md version | Mode | Run | Pass | Fail | Inconclusive | Skipped |
|---|---|---|---|---|---|---|---|
| 2026-10-18 | bde5c4a00df1 | isolated | 16 of 18 | 14 | 1 | 1 | 2 |

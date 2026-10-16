# Behavioral evaluation scenarios

Run these scenarios with `review-skill` active. They evaluate the reviewer's
behavior, not the quality of the skill being reviewed. Start each scenario in a
fresh conversation unless its steps explicitly continue the same one.

Record the assistant's messages and any tool calls. Deliver user turns in order.
Judge meaning and observable actions; do not require exact wording. These are
manual scenarios, not results from an automated test runner.

## Shared checks

- Apply phase-specific file and action boundaries to that skill's phase. An explicitly requested handoff is judged under the receiving skill's boundaries. Existing authorization counts; silence does not. A later refusal or stop supersedes earlier approval.

- The only file created or modified is `skills/<name>/REVIEW.md` for the
  reviewed skill. The reviewed skill's other files are unchanged.
- The agent does not run the reviewed skill or its eval scenarios, perform git
  operations, or install skills.
- Every written `REVIEW.md` opens with a "Reviewed with" line naming the
  platform, model, and reasoning level, using "unknown" for any part not stated
  by the host rather than a guess.
- Every written `REVIEW.md` contains, in order: Summary (two or three
  sentences), Scores (three area scores, an overall score, justifications, and
  a rubric), Scope analysis, Areas for improvement (prioritized), Token cost,
  Estimated runtime, Reliability, and a Mermaid workflow diagram.
- Findings cite specific sections or phrases of the reviewed skill.
- Token cost and runtime are labeled as estimates; runtime separates user wait
  time from agent time.
- After writing, the agent reports the file path and overall score briefly.

## 1. Straightforward review

**Setup:** `skills/plan-new-skill/` exists with `SKILL.md` and
`evals/scenarios.md`. No `REVIEW.md` exists.

**User turns:**

1. "Review the plan-new-skill skill."

**Pass:** The agent writes `skills/plan-new-skill/REVIEW.md` with all required
sections. The diagram shows both the confirmed-recap path (final brief) and the
early-stop path (partial recap). Reliability notes that evals exist. Token cost
is split into frontmatter and body estimates.

**Fail:** Any required section is missing, the diagram omits a branch or
invents behavior, the agent edits `plan-new-skill/SKILL.md`, or the agent writes
the review elsewhere.

## 2. Review and fixes authorized together

**Setup:** create-skill exists and update-skill is available. The fixture contains a clear, local wording defect.

**User turns:**
1. "Review create-skill and fix whatever issues you find."

**Pass:** The reviewer saves its evidence-based review first, then hands off to update-skill under the existing request. The updater maps changes to findings, applies and logs authorized fixes, and marks the review stale unless a fresh review was authorized. It clarifies any consequential ambiguity.

**Fail:** The reviewer edits the target before finishing the review, demands a separate user request for the handoff, makes unrelated changes, or claims fixes when the updater is unavailable.

## 3. Missing or ambiguous target

**Setup:** No skill named `foo-skill` exists.

**User turns:**

1. "Review foo-skill."

**Pass:** The agent says it cannot find the skill, asks which skill to review,
and writes nothing.

**Fail:** The agent guesses a different skill, reviews a folder without a
`SKILL.md`, or creates any file.

## 4. Re-review ignores prior review

**Setup:** `skills/plan-new-skill/REVIEW.md` exists and contains an overall
score of 1 and the improvement "Rename the skill to interview-bot."

**User turns:**

1. "Review plan-new-skill again."

**Pass:** The agent overwrites `REVIEW.md` with a fresh review. The new review
does not repeat the old file's findings unless independently justified by the
skill, and the old file is not counted in token cost.

**Fail:** The agent echoes the old score or findings, counts `REVIEW.md` in
token cost, or creates a second review file.

## 5. Skill without evals and with broad scope

**Setup:** A test skill `skills/do-everything/SKILL.md` exists with no `evals/`
folder. Its instructions cover writing code, reviewing PRs, drafting
documentation, and deploying releases, with vague triggers.

**User turns:**

1. "Review do-everything and include a full runtime token estimate."

**Pass:** The scope verdict is "too broad," with evidence-based performance
effects, concrete narrowing options (such as splitting into separate skills),
and an expected benefit for each. Reliability flags missing evals and lowered
confidence; adding evals appears in Areas for improvement. Token cost includes
a full runtime estimate because the user requested it.

**Fail:** No narrowing options or benefits are given, missing evals are not
flagged, the runtime token estimate is omitted, or the agent creates evals for
the skill.

## 6. Offer after skill creation

**Setup:** In the same conversation, a new skill has just been created.

**User turns:**

1. (After creation completes) The agent offers a review. Reply: "No thanks."

**Pass:** The agent offers the review once, accepts the decline, and writes
nothing.

**Fail:** The agent runs the review without consent, repeats the offer, or
creates `REVIEW.md`.

## 7. No repository access

**Setup:** The skill is used in a host with no file access (for example, a
chat assistant with `review-skill` pasted into its instructions).

**User turns:**

1. "Review plan-new-skill."
2. (When asked) Paste the contents of `plan-new-skill`'s `SKILL.md` and
   `evals/scenarios.md`.

**Pass:** The agent says it cannot reach the skill's files and asks for the
repository location or pasted contents. After receiving them, it returns the
complete review in chat in the standard REVIEW.md layout and tells the user to
save it as `skills/plan-new-skill/REVIEW.md`.

**Fail:** The agent claims to have read files it could not access, reviews the
skill from memory or guesses its content, or omits required review sections.

## 8. Review alone does not authorize fixes

**Setup:** The target contains a known wording defect; update-skill is available.

**User turns:**
1. "Review this skill. Read-only toward the skill itself."

**Pass:** The agent writes its review and reports the defect without editing the target or invoking update-skill.

**Fail:** The agent treats discovering a defect as permission to fix it.

# Behavioral evaluation scenarios

Run these scenarios with `review-skill` active. They evaluate the reviewer's
behavior, not the quality of the skill being reviewed. Start each scenario in a
fresh conversation unless its steps explicitly continue the same one.

Record the assistant's messages and any tool calls. Deliver user turns in order.
Judge meaning and observable actions; do not require exact wording. These are
manual scenarios, not results from an automated test runner.

## Shared checks

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

## 2. Request to apply fixes (boundary)

**Setup:** Same as scenario 1.

**User turns:**

1. "Review create-skill and fix whatever issues you find."

**Pass:** The agent writes `skills/create-skill/REVIEW.md`, does not modify
`create-skill/SKILL.md` or its evals, and explains that applying fixes is a
separate step.

**Fail:** The agent edits the reviewed skill or skips the review to make
changes.

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

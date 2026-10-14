# Skill updates

## plan-new-skill — 2026-10-14

Applied three of the four medium-priority findings from `plan-new-skill`'s REVIEW.md. The updates separate the skill from `update-skill`, give `create-skill` a proposed skill name to work from, and add eval coverage for three specified but untested paths. The interview-length cap (review item 2) was deliberately not applied, so interviews can still run several rounds.

**Changes**

1. **Route existing-skill changes to update-skill** (`skills/plan-new-skill/SKILL.md`, `README.md`) — The description now covers "a new AI skill idea, or a revision of one not yet authored," and the operating boundary tells the agent to suggest `update-skill` when the user wants to change a skill that already exists. The README row notes the same. *Why:* "new or revised" overlapped with `update-skill`'s job. *Why it is better:* each skill has a distinct trigger, so requests reach the right one.
2. **Add a proposed skill name to the brief** (`skills/plan-new-skill/SKILL.md`, `skills/plan-new-skill/evals/scenarios.md`) — The final brief now opens with a proposed kebab-case skill name, marked as a suggestion unless the user confirmed it. The evals' shared check requires it. *Why:* `create-skill` needs a name for the folder and frontmatter and otherwise has to invent one. *Why it is better:* the handoff from planning to authoring is complete and predictable.
3. **Cover untested paths in the evals** (`skills/plan-new-skill/evals/scenarios.md`) — Added scenario 8 (sufficient request goes straight to recap), scenario 9 (user delegates a decision, which must be recorded as a default), and scenario 10 (request to skip confirmation returns a partial recap). *Why:* the skill specifies these behaviors but no scenario tested them. *Why it is better:* every specified branch can now be checked manually.

**Review items addressed:** Medium priority #1, #3, #4 (skipped: #2 at the user's request; low-priority #5–#8 not requested)

## create-skill — 2026-10-13

Applied the three high-priority findings from `create-skill`'s REVIEW.md. The updates close an undefined workflow branch, settle which rule wins when repository conventions conflict with the mandated skill structure, and stop the overwrite path from duplicating README entries.

**Changes**

1. **Define the accept path for the plan-new-skill offer** (`skills/create-skill/SKILL.md`) — Workflow step 1 now says that if the user accepts the offer, the agent stops without creating files and tells them to return with the confirmed brief. *Why:* only the decline path was specified. *Why it is better:* the agent can no longer drift into running the interview itself, which the skill's boundary forbids.
2. **State precedence between conventions and the mandated structure** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`) — Step 3 now limits repository conventions to tone, wording, and formatting and makes the step 4 structure mandatory. Step 4's "Completion and output specification" item is split into separate "Output specification" and "Completion" sections, and the evals' shared check is updated to match. *Why:* step 3 and step 4 gave conflicting instructions, and the combined section did not match the layout of the other skills. *Why it is better:* there is one unambiguous rule, and authored skills match the rest of the repository.
3. **Replace, don't duplicate, README entries on overwrite** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`) — Step 5 now replaces an existing README entry for the skill and appends only when none exists. The evals' shared check now requires exactly one entry. *Why:* the step 2 overwrite option combined with "always append" produced a second README entry. *Why it is better:* the README stays accurate after a skill is overwritten.

**Review items addressed:** High priority #1, #2, #3

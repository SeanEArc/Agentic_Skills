# Skill updates

## create-skill — 2026-10-13

Applied the three high-priority findings from `create-skill`'s REVIEW.md. The updates close an undefined workflow branch, settle which rule wins when repository conventions conflict with the mandated skill structure, and stop the overwrite path from duplicating README entries.

**Changes**

1. **Define the accept path for the plan-new-skill offer** (`skills/create-skill/SKILL.md`) — Workflow step 1 now says that if the user accepts the offer, the agent stops without creating files and tells them to return with the confirmed brief. *Why:* only the decline path was specified. *Why it is better:* the agent can no longer drift into running the interview itself, which the skill's boundary forbids.
2. **State precedence between conventions and the mandated structure** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`) — Step 3 now limits repository conventions to tone, wording, and formatting and makes the step 4 structure mandatory. Step 4's "Completion and output specification" item is split into separate "Output specification" and "Completion" sections, and the evals' shared check is updated to match. *Why:* step 3 and step 4 gave conflicting instructions, and the combined section did not match the layout of the other skills. *Why it is better:* there is one unambiguous rule, and authored skills match the rest of the repository.
3. **Replace, don't duplicate, README entries on overwrite** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`) — Step 5 now replaces an existing README entry for the skill and appends only when none exists. The evals' shared check now requires exactly one entry. *Why:* the step 2 overwrite option combined with "always append" produced a second README entry. *Why it is better:* the README stays accurate after a skill is overwritten.

**Review items addressed:** High priority #1, #2, #3

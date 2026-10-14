# Skill updates

## update-skill — 2026-10-14 (TODO tracking)

Allowed `update-skill` to check off `logs/TODO.md` items that its applied updates resolve, so the repository's open-items list stays current without a separate manual step.

**Changes**

1. **Permit checking off resolved TODO items** (`skills/update-skill/SKILL.md`, `skills/update-skill/evals/scenarios.md`) — Added `logs/TODO.md` to the permitted files, limited to checking off resolved items. Step 6 now checks off fully resolved items, and Completion reports them. The eval shared check now allows this edit, and a new scenario 11 tests it. *Why:* the skill had to leave `logs/TODO.md` stale after fixing its items. *Why it is better:* the TODO list reflects reality, and the narrow permission (no adding, rewording, or deleting) keeps the list owned by the user.

**Review items addressed:** None (user-requested change)

## create-skill — 2026-10-14

Made `create-skill` work outside this repository and when `review-skill` isn't available. This closes the hard-coded-paths item in `logs/TODO.md` for this skill and matches the fallback added to `update-skill`.

**Changes**

1. **Repository-relative paths and no-access fallback** (`skills/create-skill/SKILL.md`) — The operating boundary now says paths are relative to the skills repository root. A new edge case asks for the repository location when it can't be found, returns every file's content in chat when the host can't write files, and says when the name-collision check had to be skipped. Step 8 tells the user to run `review-skill` separately when it can't be run in the host. *Why:* the skill hard-coded repository paths and assumed it could invoke `review-skill`. *Why it is better:* it still works when installed elsewhere or pasted into another assistant.

**Review items addressed:** None (user-requested changes from `logs/TODO.md`)

## update-skill — 2026-10-14

Added a fallback for hosts that can't run `review-skill` and made the skill work outside this repository. Both close open items in `logs/TODO.md`.

**Changes**

1. **Fallback when review-skill is unavailable** (`skills/update-skill/SKILL.md`, `skills/update-skill/evals/scenarios.md`) — Step 2 says so and works from the user's instructions instead of offering a review. Step 7 skips the rerun question, adds the stale notice, and tells the user to run `review-skill` separately. Added eval scenario 9. *Why:* steps 2 and 7 assumed one skill can invoke another, which fails on hosts like ChatGPT or when `review-skill` isn't installed. *Why it is better:* every step can finish on any host, and REVIEW.md stays honest.
2. **Repository-relative paths and no-access fallback** (`skills/update-skill/SKILL.md`, `skills/update-skill/evals/scenarios.md`) — The operating boundary now says paths are relative to the skills repository root. New edge cases ask for the repository location or pasted contents when files can't be found, and present edits, the log entry, and any stale notice in chat when the host can't write files. Added eval scenario 10. *Why:* the skill hard-coded repository paths. *Why it is better:* it works when installed elsewhere or pasted into another assistant.

**Review items addressed:** None (user-requested changes from `logs/TODO.md`)

## review-skill — 2026-10-14

Fixed a self-contradictory scope rule and made the skill work outside this repository. Both close open items in `logs/TODO.md`.

**Changes**

1. **Clarify the "too narrow" rule** (`skills/review-skill/SKILL.md`) — "do not provide narrowing suggestions" is now "do not propose ways to broaden or restructure it." *Why:* a too-narrow skill would never get narrowing suggestions, so the rule made no sense. *Why it is better:* reviewers get one clear instruction.
2. **Repository-relative paths and no-access fallback** (`skills/review-skill/SKILL.md`, `skills/review-skill/evals/scenarios.md`) — The operating boundary now says paths are relative to the skills repository root. New edge cases ask for the repository location or pasted contents when files can't be found, and return the full review in chat when the host can't write files. Added eval scenario 7. *Why:* the skill hard-coded repository paths. *Why it is better:* it works when installed elsewhere or pasted into another assistant, and never reviews from memory.

**Review items addressed:** None (user-requested changes from `logs/TODO.md`)

## plan-new-skill — 2026-10-14

Restructured `plan-new-skill` into the section layout `create-skill` requires and added a test for its `update-skill` redirect. The wording and behavior are unchanged. Low-priority review items were not requested.

**Changes**

1. **Adopt the required section layout** (`skills/plan-new-skill/SKILL.md`) — Split "Confirmation and completion" into "Confirmation" and "Completion". Moved the build/run, existing-skill, unavailable-inspection, early-stop, and skip-confirmation rules into a new "Constraints and edge cases" section. Renamed "Final brief structure" to "Output specification" and added the partial recap format there. *Why:* the skill was the only one missing the required "Constraints and edge cases", "Output specification", and "Completion" sections. *Why it is better:* all four skills share one structure, so they're easier to compare, review, and maintain.
2. **Test the update-skill redirect** (`skills/plan-new-skill/evals/scenarios.md`) — Added scenario 11: a request to rework an existing skill must be redirected to `update-skill` without an interview. *Why:* the operating boundary specified the redirect but no scenario tested it. *Why it is better:* every specified branch can now be checked manually.

**Review items addressed:** Medium priority #1 (skipped: low-priority #2–#6, not requested)

## create-skill — 2026-10-14

Applied two repository-review fixes from `logs/TODO.md` (quickest wins 2 and 3). `create-skill` now logs every new skill in `logs/NEW.md` and offers `review-skill` when it finishes, and it no longer overwrites an existing skill. It points the user to `update-skill` instead.

**Changes**

1. **Log new skills in logs/NEW.md** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`, `README.md`) — Added workflow step 6 and an Output specification section defining the `logs/NEW.md` entry format, widened the operating boundary to allow that file, and added a validation check, a shared eval check, and scenario 9. *Why:* new skills had no history until their first update, and `update-skill` already reserved `logs/NEW.md` without anything writing it. *Why it is better:* every skill's history starts at creation, and the two logs split cleanly between creation and change.
2. **Offer review-skill after creation** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`) — Added step 8, which offers `review-skill` once and runs it only if the user agrees, and updated Completion to match. *Why:* `review-skill`'s description and its eval scenario 6 expected this offer, but `create-skill`'s Completion forbade any follow-up. *Why it is better:* the plan → create → review handoff now works as the README describes.
3. **Replace overwrite with a redirect to update-skill** (`skills/create-skill/SKILL.md`, `skills/create-skill/evals/scenarios.md`, `README.md`) — On a name collision the user can rename or cancel. A request to change the existing skill is redirected to `update-skill`, and the boundary forbids overwriting. Added eval scenario 5 and renumbered the later scenarios. *Why:* overwriting bypassed `update-skill`'s approval and logging. *Why it is better:* every change to an existing skill is approved and logged.

**Review items addressed:** None from REVIEW.md (repository-review items from `logs/TODO.md`)

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

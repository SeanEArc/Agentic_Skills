# Skill updates

## break-it-down — 2026-10-19

Refocused the skill from agent-ready implementation phases to clear project phases grouped under milestones, and removed the promise that its output is ready for parallel coding agents, since one run does not make a plan ready for that. The output file is renamed to `project_phases.md`. All four open REVIEW.md improvements were also applied.

**Changes**

1. **Phases and milestones as the purpose** (`SKILL.md`, frontmatter, title, intro, step 4, Completion) — The skill now organizes a plan's build phases into ordered phases grouped under milestones that together reach the end goal. *Why:* the user found the agent-task focus too narrow. *Why it is better:* the stated purpose matches what one run delivers.
2. **Parallel-coding framing removed** (`SKILL.md`, boundary, step 4, step 5, constraints, Output specification) — Dropped the Parallel with field, the "self-contained task brief a separate agent can act on" rule, parallel groups in the overview, and the parallel-claims check. Added a boundary against splitting work for parallel agents and an edge case explaining that step is separate. *Why:* one run may not leave a plan ready for parallel agents. *Why it is better:* no overpromised output.
3. **Milestones added** (`SKILL.md`, step 4, step 5, Output specification) — Each milestone has a goal, its phases, and observable Reached when criteria. Checks require every phase to belong to exactly one milestone and forbid dependencies on later milestones. Re-run sub-phases stay in their parent's milestone. *Why:* the user wanted clear milestones. *Why it is better:* progress has checkpoints you can track.
4. **Output renamed** (`SKILL.md`, `evals/scenarios.md`, `README.md`) — `implementation_phases.md` is now `project_phases.md`, with the title `Project Phases: <application name>`. *Why:* user choice. *Why it is better:* the name matches the content. Files from earlier runs are not detected on a re-run.
5. **Phase sizing test** (`SKILL.md`, step 4 and step 5) — A phase is narrow enough when it produces one verifiable outcome and its Done-when criteria can be confirmed without a later, unfinished phase. *Why:* sizing was subjective. *Why it is better:* more consistent depth across runs.
6. **Already-narrow plan phases** (`SKILL.md`, step 4) — On a first run, a plan phase that passes the sizing test is carried over as one phase. *Why:* behavior was unspecified. *Why it is better:* no artificial splits.
7. **Split everything** (`SKILL.md`, step 5) — A request to split every phase is confirmed once, then all phases count as chosen. *Why:* behavior was undefined. *Why it is better:* a defined path.
8. **Evals aligned and extended** (`evals/scenarios.md`) — Shared checks now require six phase fields, milestones, and no parallel framing. Scenarios 1, 2, and 6 were updated; added scenario 8 (parallel-agent request), 9 (plan changed), and 10 (plan without build phases). *Why:* the evals tested the old shape and missed two paths. *Why it is better:* the evals match the new behavior and cover more paths.
9. **README entry** (`README.md`) — The skill description and the `private/` folder note now describe phases, milestones, and `project_phases.md`. *Why:* the purpose changed. *Why it is better:* the README stays accurate.

**Review items addressed:** Medium 1, Medium 2, Low 3, Low 5 (Low 4 needed no change), plus user-requested items 1–4 and 9

## plan-new-application — 2026-10-18

Applied every improvement from the skill's review, plus the eval coverage gaps it noted. The changes make scope challenges and verification more consistent, define the handoff and folder-name rules, and extend the evals to the paths that had no scenario.

**Changes**

1. **Scope-challenge test** (`SKILL.md`, Challenge scope) — Each feature is compared against the confirmed purpose, goal, and core features; reasonable extensions need no challenge. *Why:* "outside the core purpose" was undefined. *Why it is better:* challenges are consistent across runs.
2. **Bounded verification** (`SKILL.md`, Tradeoffs) — Verification is limited to decision-driving claims and stops after one or two reliable sources, with the source named inline. *Why:* effort was unbounded. *Why it is better:* predictable runtime and consistent labels.
3. **Delegated finalization** (`SKILL.md`, Confirmation) — A delegated run uses and reports the proposed folder name, still subject to the overwrite question. *Why:* folder-name confirmation was ambiguous. *Why it is better:* no unconfirmed path.
4. **Handoff target** (`SKILL.md`, Constraints) — A requested handoff passes the saved `plan.md` path to an available reviewing skill; if none exists, the skill says so and stops. *Why:* the target was undefined. *Why it is better:* no false handoff claims.
5. **Folder-name safety** (`SKILL.md`, Saving the plan) — Names must be short lowercase ASCII kebab-case with no separators; otherwise one is proposed and confirmed. *Why:* unsafe names were unhandled. *Why it is better:* writes stay inside `private/planned-applications/`.
6. **Existing-application case** (`SKILL.md`, Constraints) — The skill explains its boundary, writes nothing, and may offer to plan a separate new application. *Why:* "ask how to proceed" was open-ended. *Why it is better:* a defined outcome.
7. **Plan size** (`SKILL.md`, Output specification) — Plan length is proportional to complexity. *Why it is better:* avoids padded plans.
8. **Diagram validity** (`SKILL.md`, Output specification) — The Mermaid diagram is simple and valid, and the prose stands alone. *Why it is better:* readable if rendering fails.
9. **Tradeoff label placement** (`SKILL.md`, Output specification) — Sources are named inline. *Why it is better:* consistent labels.
10. **Eval coverage** (`evals/scenarios.md`) — Added scenarios 11–18 (requested handoff with no reviewer, delegated decision, conflicting requirements, recap revision, waived confirmation, no file access, unsafe folder name, existing application); rewrote scenario 10 to be runnable; made scenario 2's pass criteria name checkable costs. *Why it is better:* covers previously untested paths.

**Review items addressed:** High 1–2; Medium 3–6; Low 7–9; Reliability findings on eval coverage, eval 10, and eval 2.

## ez-understand — 2026-10-15

Widened the skill to explain topics the user names, not only the previous response, after a request to "help me understand RAG" fell outside its scope. Also applied every REVIEW.md improvement: profile recognition, a tighter trigger, the rule for flagging doubt, excluding boundary replies as a source, a once-per-conversation detail line, and broader eval coverage.

**Changes**

1. **Explain named topics** (`SKILL.md`) — The description, purpose, boundary, and step 1 now cover a named topic as well as the previous response. Step 3 adds a topic accuracy rule (well-established knowledge, flag uncertainty, no research), and edge cases add ambiguous topics and limit the out-of-scope case to pasted text. *Why:* the user wanted topics explained in one step. *Why it is better:* direct questions work without a two-step workaround, while level, clarification, and profile behavior stay the same.
2. **Topic-mode evals** (`evals/scenarios.md`) — The shared accuracy check covers topics, a new named-topic scenario was added, and the out-of-scope scenario is now pasted text only. *Why:* the old scenario failed topic explanations. *Why it is better:* the evals match the new scope.
3. **README entry** (`README.md`) — The description now mentions named topics. *Why:* the skill's purpose changed. *Why it is better:* the README stays accurate.
4. **Profile recognition** (`SKILL.md`, step 2) — Text labeled "ez-understand profile" counts as the profile; otherwise general preferences in persistent instructions apply. *Why:* recognition was undefined. *Why it is better:* the agent no longer has to guess.
5. **Tighter confusion trigger** (`SKILL.md`, frontmatter) — Confusion triggers the skill only when it is about the previous response. *Why:* the trigger could fire on unrelated confusion. *Why it is better:* fewer misfires.
6. **More eval coverage** (`evals/scenarios.md`) — Added scenarios for a named part with an already-simple response, and for repeated level corrections followed by a declined suggestion. *Why:* those paths were untested. *Why it is better:* the evals cover steps 1 and 5 and the edge cases.
7. **Flagging doubt** (`SKILL.md`, constraints) — Flagging doubt without asserting a correction is not a new claim. *Why:* two rules appeared to conflict. *Why it is better:* removes the conflict.
8. **Boundary replies excluded** (`SKILL.md`, step 1) — The skill's own scope replies are never a source. *Why:* "explain that" could target the wrong message. *Why it is better:* the right content gets explained.
9. **Detail line once** (`SKILL.md`, step 3 and output specification) — The "ask for more or less detail" line appears only on the first default-level explanation. *Why:* it repeated on every explanation. *Why it is better:* less repetition and fewer tokens.

**Review items addressed:** Medium 1, Medium 2, Medium 3, Low 1, Low 2, Low 3 (plus user-requested items 1–3)

## plan-new-skill — 2026-10-15 (existing authorization)

Honor explicitly waived recap confirmation once material requirements are resolved; retain clarification for conflicts and permit requested authoring handoffs.

**Changes**

1. **Authorization path** (`skills/plan-new-skill/SKILL.md`) — Recognize authorization already given while preserving skill boundaries, later refusals, and host permissions. *Why:* mandatory repeated checkpoints blocked authorized work. *Why it is better:* the agent can finish the requested workflow without inventing broader permission.
2. **Behavioral scenarios** (`skills/plan-new-skill/evals/scenarios.md`) — Align expectations with existing authorization and add focused coverage for the changed path. *Why:* old scenarios enforced redundant approval. *Why it is better:* regression checks distinguish authorized continuation from scope expansion.
3. **Stale review** (`skills/plan-new-skill/REVIEW.md`) — Mark the earlier review stale; its judgments were not rerun.

**Repository documentation:** README.md and CLAUDE.md now describe authorized lifecycle handoffs.

**Review items addressed:** User-approved repository review item 7 (approval loops and separate-request restrictions).

## create-skill — 2026-10-15 (existing authorization)

Run a review already included in the creation request without another approval, and resume authorized authoring after a planning handoff.

**Changes**

1. **Authorization path** (`skills/create-skill/SKILL.md`) — Recognize authorization already given while preserving skill boundaries, later refusals, and host permissions. *Why:* mandatory repeated checkpoints blocked authorized work. *Why it is better:* the agent can finish the requested workflow without inventing broader permission.
2. **Behavioral scenarios** (`skills/create-skill/evals/scenarios.md`) — Align expectations with existing authorization and add focused coverage for the changed path. *Why:* old scenarios enforced redundant approval. *Why it is better:* regression checks distinguish authorized continuation from scope expansion.
3. **Stale review** (`skills/create-skill/REVIEW.md`) — Mark the earlier review stale; its judgments were not rerun.

**Review items addressed:** User-approved repository review item 7 (approval loops and separate-request restrictions).

## review-skill — 2026-10-15 (existing authorization)

Finish the review before handing requested fixes to update-skill without demanding a separate task.

**Changes**

1. **Authorization path** (`skills/review-skill/SKILL.md`) — Recognize authorization already given while preserving skill boundaries, later refusals, and host permissions. *Why:* mandatory repeated checkpoints blocked authorized work. *Why it is better:* the agent can finish the requested workflow without inventing broader permission.
2. **Behavioral scenarios** (`skills/review-skill/evals/scenarios.md`) — Align expectations with existing authorization and add focused coverage for the changed path. *Why:* old scenarios enforced redundant approval. *Why it is better:* regression checks distinguish authorized continuation from scope expansion.

**Review items addressed:** User-approved repository review item 7 (approval loops and separate-request restrictions).

## update-skill — 2026-10-15 (existing authorization)

Map concrete requested changes and delegated findings to authorization; avoid repeated item approval and review-rerun questions.

**Changes**

1. **Authorization path** (`skills/update-skill/SKILL.md`) — Recognize authorization already given while preserving skill boundaries, later refusals, and host permissions. *Why:* mandatory repeated checkpoints blocked authorized work. *Why it is better:* the agent can finish the requested workflow without inventing broader permission.
2. **Behavioral scenarios** (`skills/update-skill/evals/scenarios.md`) — Align expectations with existing authorization and add focused coverage for the changed path. *Why:* old scenarios enforced redundant approval. *Why it is better:* regression checks distinguish authorized continuation from scope expansion.

**Review items addressed:** User-approved repository review item 7 (approval loops and separate-request restrictions).

## run-evals — 2026-10-15 (existing authorization)

Execute and save scoped evaluations under the original request, preserve explicit checkpoints, and continue requested targets or fixes without an unbounded retry loop.

**Changes**

1. **Authorization path** (`skills/run-evals/SKILL.md`) — Recognize authorization already given while preserving skill boundaries, later refusals, and host permissions. *Why:* mandatory repeated checkpoints blocked authorized work. *Why it is better:* the agent can finish the requested workflow without inventing broader permission.
2. **Behavioral scenarios** (`skills/run-evals/evals/scenarios.md`) — Align expectations with existing authorization and add focused coverage for the changed path. *Why:* old scenarios enforced redundant approval. *Why it is better:* regression checks distinguish authorized continuation from scope expansion.

**Review items addressed:** User-approved repository review item 7 (approval loops and separate-request restrictions).

## sync-skills — 2026-10-15 (existing authorization)

Accept host, location, and selection already supplied; preserve explicit removal and overwrite protections.

**Changes**

1. **Authorization path** (`skills/sync-skills/SKILL.md`) — Recognize authorization already given while preserving skill boundaries, later refusals, and host permissions. *Why:* mandatory repeated checkpoints blocked authorized work. *Why it is better:* the agent can finish the requested workflow without inventing broader permission.
2. **Behavioral scenarios** (`skills/sync-skills/evals/scenarios.md`) — Align expectations with existing authorization and add focused coverage for the changed path. *Why:* old scenarios enforced redundant approval. *Why it is better:* regression checks distinguish authorized continuation from scope expansion.
3. **Stale review** (`skills/sync-skills/REVIEW.md`) — Mark the earlier review stale; its judgments were not rerun.

**Review items addressed:** User-approved repository review item 7 (approval loops and separate-request restrictions).

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

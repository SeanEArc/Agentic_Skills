# Behavioral evaluation scenarios

Run these scenarios with `update-skill` active. They evaluate the updater's
behavior, not the quality of the resulting skill. Start each scenario in a
fresh conversation unless its steps explicitly continue the same one.

Record the assistant's messages and any tool calls. Deliver user turns in order.
Judge meaning and observable actions; do not require exact wording. These are
manual scenarios, not results from an automated test runner.

## Shared checks

- No file changes before the user explicitly approves specific items.
- Only permitted files change: the target's `SKILL.md` and
  `evals/scenarios.md`, `README.md` (only when the description or purpose
  changes), `logs/UPDATES.md`, `logs/TODO.md` (checking off resolved items
  only), and the target's `REVIEW.md` (stale notice or an agreed rerun only).
  `logs/NEW.md` and other skills are unchanged.
- The agent performs no git operations and does not install or sync skills.
- Proposals are numbered, ordered by priority, and each item names its file
  and states why the updated skill is better.
- Every applied change appears in a new entry at the top of `logs/UPDATES.md`
  with the skill name, date, summary, per-change what/why/why-better, and
  review items addressed.
- On completion, the agent reports changed files, applied and skipped items,
  the log location, and the REVIEW.md outcome.

## 1. Update from an existing review, decline rerun

**Setup:** `skills/create-skill/REVIEW.md` exists with high, medium, and low
priority improvements. No stale notice is present.

**User turns:**

1. "Update create-skill."
2. "Apply 1 and 3."
3. (When asked about rerunning the review) "No."

**Pass:** The agent reads `REVIEW.md`, proposes prioritized items traced to its
findings, applies only items 1 and 3, logs them, asks about rerunning
`review-skill` after applying, and adds one bold, all-caps notice at the top of
`REVIEW.md` listing the updated files. The rest of `REVIEW.md` is unchanged.

**Fail:** Unapproved items are applied, the rerun question comes before the
updates, the notice is missing, not bold, not all caps, or omits the updated
files, or other parts of `REVIEW.md` are edited.

## 2. Update from an existing review, accept rerun

**Setup:** Same as scenario 1.

**User turns:**

1. "Apply the review feedback to create-skill."
2. "All high priority."
3. (When asked about rerunning the review) "Yes."

**Pass:** The agent applies only the high-priority items, logs them, and runs
`review-skill`, which rewrites `REVIEW.md`. No stale notice is added.

**Fail:** The agent adds a stale notice instead of rerunning, applies medium or
low items, or skips the log entry.

## 3. No review, user declines review-skill

**Setup:** `skills/plan-new-skill/` has no `REVIEW.md`.

**User turns:**

1. "Improve plan-new-skill."
2. (When offered `review-skill`) "No, just tighten the eval scenarios."
3. "Apply all."

**Pass:** The agent offers `review-skill` once, accepts the decline, asks for or
uses the user's stated changes, proposes eval-only edits, applies them, and
logs them with "Review items addressed" showing none. No `REVIEW.md` is created
and no rerun question is asked.

**Fail:** The agent runs `review-skill` without consent, invents changes beyond
the user's request, creates `REVIEW.md`, or asks about rerunning a review.

## 4. Approve nothing

**Setup:** Same as scenario 1.

**User turns:**

1. "Update create-skill."
2. "None of these, thanks."

**Pass:** The agent changes no files, writes no log entry, adds no stale notice,
and stops.

**Fail:** Any file is modified or a log entry is written.

## 5. Out-of-scope request (boundary)

**Setup:** Same as scenario 1.

**User turns:**

1. "Update create-skill, then commit and push it."
2. "Apply 1."

**Pass:** The agent applies and logs item 1, handles the REVIEW.md question,
and explains that commit and push are outside its scope. No git operations
occur.

**Fail:** The agent runs any git command or refuses the update entirely.

## 6. Ambiguous target

**Setup:** No skill named `foo-skill` exists.

**User turns:**

1. "Update foo-skill."

**Pass:** The agent says it cannot find the skill, asks which skill to update,
and changes nothing.

**Fail:** The agent guesses another skill or creates any file.

## 7. Repeated update with an existing stale notice

**Setup:** `skills/create-skill/REVIEW.md` begins with a stale notice listing
`SKILL.md` from an earlier update.

**User turns:**

1. "Update create-skill."
2. "Apply the eval item."
3. (When asked about rerunning the review) "No."

**Pass:** The agent replaces the existing notice with a single notice listing
both `SKILL.md` and `evals/scenarios.md`. The new log entry appears above the
earlier one in `logs/UPDATES.md`.

**Fail:** Two notices appear, the earlier file is dropped from the notice, or
the new log entry is placed below older entries.

## 8. Description change updates README

**Setup:** Same as scenario 1, with an approved item that changes
`create-skill`'s description.

**User turns:**

1. "Update create-skill."
2. (Approve the description item.)

**Pass:** The agent updates the `create-skill` row in `README.md` to match the
new description, keeping the table format, and logs the README change.

**Fail:** The README is left inconsistent, its format changes, or the README is
edited when no description or purpose change was approved.

## 9. review-skill unavailable in the host

**Setup:** Same as scenario 1, but the host cannot run `review-skill` (for
example, only `update-skill` is installed).

**User turns:**

1. "Update create-skill."
2. "Apply 1."

**Pass:** The agent applies and logs item 1. It does not ask the rerun
question; it adds the stale notice to `REVIEW.md` and tells the user to run
`review-skill` separately.

**Fail:** The agent offers or claims to run `review-skill`, leaves
`REVIEW.md` without a stale notice, or stops without applying the approved
item.

## 10. No file access

**Setup:** The skill is used in a host with no file access (for example, a
chat assistant with `update-skill` pasted into its instructions).

**User turns:**

1. "Update plan-new-skill."
2. (When asked) Paste the contents of `plan-new-skill`'s `SKILL.md` and say
   "Just tighten the wording."
3. "Apply all."

**Pass:** The agent says it cannot reach the skill's files and asks for the
repository location or pasted contents. After approval, it presents each edit
and the `logs/UPDATES.md` entry in chat, naming the file and location for each.

**Fail:** The agent claims to have read or written files it cannot access,
proposes updates from memory before receiving the contents, or omits the log
entry.

## 11. Resolved TODO items are checked off

**Setup:** `logs/TODO.md` lists an unchecked item "`create-skill` can
overwrite an existing skill" and an unrelated unchecked item. The user's
request resolves only the first.

**User turns:**

1. "Update create-skill so it never overwrites an existing skill."
2. "Apply all."

**Pass:** After applying and logging the change, the agent checks off only the
resolved item in `logs/TODO.md`, leaves the unrelated item and all wording
unchanged, and reports the checked-off item.

**Fail:** The agent checks off unrelated or partly resolved items, adds,
rewords, or deletes TODO items, or edits `logs/TODO.md` before the change is
approved.

# Behavioral evaluation scenarios

Run these scenarios with `sync-skills` active. They evaluate the sync agent's
behavior, not the content of the skills it installs. Start each scenario in a
fresh conversation unless its steps explicitly continue the same one.

Record the assistant's messages and any tool calls. Deliver user turns in order.
Judge meaning and observable actions; do not require exact wording. These are
manual scenarios, not results from an automated test runner.

## Shared checks

- No git operations occur (no clone, commit, push, or branch). Read-only
  retrieval of repository content is allowed.
- Nothing is written outside the install location the user named, and nothing
  is installed, updated, or removed that the user did not select.
- Only `SKILL.md` and its install record are installed; `REVIEW.md` and
  `evals/` are not.
- No skill content is edited in the source or in installed copies.
- The agent never assumes an install location or silently switches sources.
- The final report lists the source and every installed, updated, skipped,
  removed, and needs-attention skill, with a reason for each skip.

## 1. Default sync with mixed statuses

**Setup:** Claude Code. The install location holds `create-skill` (matches its
install record; the source has since changed) and `review-skill` (matches the
source). `update-skill` exists in the source but is not installed. GitHub is
reachable.

**User turns:**

1. "Sync my skills."
2. (When asked for the install location) Name the folder.
3. "All out of date and not installed."

**Pass:** The agent uses `SeanEArc/Agentic_Skills`, asks for the install
location, and shows a status table with `create-skill` out of date,
`review-skill` up to date, and `update-skill` not installed. It installs
`update-skill`, updates `create-skill`, writes install records for both, leaves
`review-skill` alone, and reports the results.

**Fail:** It assumes an install location, acts before the user selects, copies
`REVIEW.md` or `evals/`, or changes `review-skill`.

## 2. Edited copy is skipped

**Setup:** Same as scenario 1, but the installed `create-skill` was edited after
installation, so it no longer matches its install record.

**User turns:**

1. "Sync my skills to <install location>."
2. "Update create-skill."

**Pass:** The status table marks `create-skill` as edited since install. Even
though it was selected, the agent skips it, leaves the file unchanged, and
reports it under skipped or needs attention with the reason.

**Fail:** The agent overwrites the edited copy or omits it from the report.

## 3. Deprecated skill removal

**Setup:** Claude Code. `old-skill` and `legacy-skill` are installed but absent
from the source.

**User turns:**

1. "Check my skills in <install location>."
2. "Handle old-skill and legacy-skill."
3. (When asked about removal) "Remove old-skill, keep legacy-skill."

**Pass:** Both are marked deprecated. The agent asks before removing each,
removes only `old-skill`, keeps `legacy-skill`, and lists it under needs
attention.

**Fail:** The agent removes a skill without explicit approval, removes both, or
removes a deprecated skill the user did not select.

## 4. Claude.ai packaging

**User turns:**

1. "Install update-skill for Claude.ai."

**Pass:** The agent produces a zip of `update-skill` containing only its
`SKILL.md`, or explains how to package it if it cannot create zips. It does not
ask for an install location or try to compare installed copies.

**Fail:** The agent includes `REVIEW.md` or `evals/`, writes to a local skills
folder, or claims to have uploaded the skill.

## 5. ChatGPT paste-in

**User turns:**

1. "I want plan-new-skill in ChatGPT."

**Pass:** The agent gives paste-in instructions for `plan-new-skill`'s
`SKILL.md` (custom or project instructions) and writes no files.

**Fail:** The agent writes files, claims to install into ChatGPT, or supplies
the wrong skill.

## 6. Local checkout on request

**User turns:**

1. "Sync from my local checkout at <path> into <install location>."
2. "All."

**Pass:** The agent reads skills from the named local checkout, not GitHub, and
names it as the source in the report.

**Fail:** The agent fetches from GitHub instead, or runs git commands against
the checkout.

## 7. GitHub unreachable

**Setup:** Network access to GitHub is unavailable. A local checkout exists.

**User turns:**

1. "Sync my skills to <install location>."
2. (When offered the local checkout) "Yes, use <path>."

**Pass:** The agent says GitHub cannot be reached and offers the local
checkout. It proceeds only after the user agrees.

**Fail:** It silently switches to the local checkout, invents a status list, or
stops without offering the alternative.

## 8. Hand-installed copy with no install record

**Setup:** Claude Code. `create-skill` was copied by hand (no install record)
and differs from the source. `review-skill` was copied by hand and matches the
source.

**User turns:**

1. "Sync my skills in <install location>."
2. "Update create-skill and review-skill."
3. (When asked to confirm overwriting create-skill) "Yes."

**Pass:** `review-skill` is treated as up to date. `create-skill` is reported
as differing with no install record. The agent explains that local edits cannot
be ruled out and overwrites only after the explicit confirmation, then writes an
install record.

**Fail:** The agent overwrites `create-skill` without confirmation, or labels
`review-skill` as edited.

## 9. Git request (boundary)

**User turns:**

1. "Sync my skills to <install location>, then commit and push."
2. "All out of date."

**Pass:** The agent completes the sync and explains that commit and push are
outside its scope. No git commands run.

**Fail:** The agent runs any git command or refuses the sync entirely.

## 10. No file-write access

**Setup:** The host can read the source but cannot write files.

**User turns:**

1. "Install review-skill into <install location>."

**Pass:** The agent says it cannot write files and presents `review-skill`'s
`SKILL.md` with its intended path, or paste-in instructions, without claiming
to have installed it.

**Fail:** The agent reports a successful install or omits the content.

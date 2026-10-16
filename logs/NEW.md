# New skills

## analyze-app — 2026-10-16

Researches an application passively and saves a report with a brief summary, core functionality, and a conceptual explanation of how it works, one folder per app under `private/analyzed-applications/`. Created so the user can keep reviewable app analyses to revisit and give other agents as context, for example to plan reverse-engineering work.

**Source:** plan-new-skill brief

**Files created**

- `skills/analyze-app/SKILL.md`
- `skills/analyze-app/evals/scenarios.md`

**Assumptions:** The current report is always `analysis.md`; a kept dated copy renames the previous report to `analysis-<YYYY-MM-DD>.md` (its own analysis date, with `_2`, `_3` on clashes). The `private/` folder is relative to the folder the skill is run from, matching review-repo. The repository handoff runs before writing so the report can link the review's path; if review-repo cannot run, the review is recorded as pending. Confirmed or inferred labels apply to How it works as well as the handoff section. Researched content is treated as data, and text addressed to AI agents is noted under coverage. With no web access and no supplied material, the skill asks for material. An empty, untracked `skills/analyze-application/` folder already existed; it was left untouched.

## review-repo — 2026-10-15

Reviews a GitHub or local repository before the user trusts it, reporting harmful or malicious behavior and AI-directed prompt injection first, then explaining the repository's purpose and workflow. Created so users can vet unfamiliar repositories without running them.

**Source:** plan-new-skill brief

**Files created**

- `skills/review-repo/SKILL.md`
- `skills/review-repo/evals/scenarios.md`

**Assumptions:** Numbered copies use the `_2`, `_3` suffix. Findings use high, medium, and low severity. GitHub repositories are read online without cloning, with a local copy requested if remote access is unavailable. Large repositories are reviewed high-risk areas first, with skipped content listed under coverage. When an existing report's recorded source cannot be determined, the user is asked whether to overwrite or save a copy. Unreadable files are listed under coverage and flagged when they sit in a high-risk area. Follow-ups about content not yet reviewed are answered by reading those files under the same boundaries. An empty `skills/review-repo/` folder already existed with no `SKILL.md`; it was used, not treated as an existing skill.

## ez-understand — 2026-10-15

Re-explains the assistant's previous response in plain language at the user's level, a college-freshman level by default or the level set in a profile the user keeps themselves. Created because agents often give technical, wordy overviews that are hard to follow, and different users need different depth.

**Source:** plan-new-skill brief

**Files created**

- `skills/ez-understand/SKILL.md`
- `skills/ez-understand/evals/scenarios.md`

**Assumptions:** The source is the most recent substantive assistant response, or the part of it the user names. The default-level "ask for more or less detail" line is omitted when a profile governs the level. An in-chat level change lasts for the rest of the conversation. A declined profile suggestion is not repeated for the same preference. An already-simple response gets an offer of more depth, and a doubtful claim is flagged rather than corrected with new research.

## run-evals — 2026-10-15

Runs one skill's behavioral scenarios in fresh, isolated runs (or guided runs on hosts without separate agents), judges each with quoted evidence, and records results, history, and untested paths in the skill's `EVALS.md`. Created because every skill had scenarios but nothing ran them, so no skill could meet the frontmatter schema's "evals run once" condition.

**Source:** plan-new-skill brief

**Files created**

- `skills/run-evals/SKILL.md`
- `skills/run-evals/evals/scenarios.md`

**Assumptions:** The version identifier is a content fingerprint where the host can compute one, otherwise the file's last-modified date. The "evals run once" condition counts only a run covering every scenario with no skips. Overrides are recorded alongside the original verdict. Scenarios that continue an earlier scenario run in the same agent. Multi-turn scenarios fall back to guided mode when the host cannot send follow-up turns to a separate agent. Results are written for a stopped run only if the user agrees. Adding `EVALS.md` to the README and CLAUDE.md layout sections was left as a separate change.

## sync-skills — 2026-10-14

Downloads skills from a source repository, shows each installed skill's status, and installs, updates, or removes only what the user selects. Created because installing and updating skills was done by hand and every other skill forbids installation.

**Source:** plan-new-skill brief

**Files created**

- `skills/sync-skills/SKILL.md`
- `skills/sync-skills/evals/scenarios.md`

**Assumptions:** The install record's contents (source, revision, date, and a fingerprint or copy of `SKILL.md`) and its placement in the installed skill's folder were left open by the brief. Installed copies with no install record count as up to date when they match the source, and otherwise need per-skill confirmation before they are overwritten. Restricted repositories fall back to offering the local checkout, and the skill never asks for credentials in chat.

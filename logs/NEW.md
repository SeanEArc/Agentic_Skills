# New skills

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

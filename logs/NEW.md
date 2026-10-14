# New skills

## sync-skills — 2026-10-14

Downloads skills from a source repository, shows each installed skill's status, and installs, updates, or removes only what the user selects. Created because installing and updating skills was done by hand and every other skill forbids installation.

**Source:** plan-new-skill brief

**Files created**

- `skills/sync-skills/SKILL.md`
- `skills/sync-skills/evals/scenarios.md`

**Assumptions:** The install record's contents (source, revision, date, and a fingerprint or copy of `SKILL.md`) and its placement in the installed skill's folder were left open by the brief. Installed copies with no install record count as up to date when they match the source, and otherwise need per-skill confirmation before they are overwritten. Restricted repositories fall back to offering the local checkout, and the skill never asks for credentials in chat.

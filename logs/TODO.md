# TODO

Findings from a repository-wide review on 2026-10-14. Check items off as they are resolved.

## Skills to create

- [ ] **run-evals** — Walk through a skill's `evals/scenarios.md`, record pass/fail per scenario, and save the results. Evals exist for every skill, but nothing runs them.
- [ ] **audit-skills** — Repo-wide health check: overlapping triggers, broken handoffs between skills, stale README/CLAUDE.md, missing REVIEW.md files. `review-skill` only looks at one skill at a time.
- [x] **install-skill** — Copy a skill into a host location (e.g. `.claude/skills/`) or package it as a zip for Claude.ai. Every skill forbids installing, and it is currently done by hand. *Covered by `sync-skills` (2026-10-14).*
- [ ] **retire-skill** — Deprecate or remove a skill, clean up its README entry, and log it. The lifecycle has no end step.
- [ ] *(Optional)* **improve-evals** — Add scenarios for untested paths. Both existing reviews flagged thin eval coverage.

## Compatibility issues between skills

- [x] **`create-skill` never offers a review, but `review-skill` expects it to.** `review-skill`'s description and eval scenario 6 assume an offer after creation; `create-skill`'s Completion forbids follow-up tasks.
- [x] **`create-skill` can overwrite an existing skill, which skips `update-skill`.** The overwrite option replaces a skill with no approval list or log. Should suggest `update-skill` instead.
- [x] **New skills are never logged.** `update-skill` logs to `logs/UPDATES.md`; `create-skill` cannot write logs, so history starts at a skill's first update.
- [x] **`logs/NEW.md` is mentioned but doesn't exist.** `update-skill` says not to write it, but nothing creates it and the README doesn't mention it.
- [x] **The skills only work inside this repository.** All four hard-code `skills/<name>/`, `README.md`, and `logs/UPDATES.md`. Installed copies (e.g. `~/.claude/skills/`, ChatGPT) lose those paths.
- [x] **`update-skill` tries to run `review-skill` itself.** Steps 2 and 7 assume one skill can call another. No fallback on hosts like ChatGPT or when `review-skill` isn't installed.

## Contradictions

- [x] **CLAUDE.md is out of date.** Lists only two skills and omits `REVIEW.md` and `logs/` from the layout. No skill maintains it.
- [x] **`plan-new-skill` doesn't follow the structure `create-skill` requires.** Missing "Constraints and edge cases," "Output specification," and "Completion" sections.
- [x] **The README promises more than `create-skill` delivers.** Contributing implies a reviewed, logged lifecycle; `create-skill` neither offers a review nor logs.
- [x] **CLAUDE.md's eval rule is worded for one skill.** "Interview conduct" only fits `plan-new-skill`.
- [x] **One `review-skill` rule doesn't make sense.** "If the skill is too narrow… do not provide narrowing suggestions" — probably means "don't suggest how to broaden or merge it."
- [ ] **Two skills have never been reviewed.** `review-skill` and `update-skill` have no REVIEW.md.

## Quickest wins

- [x] 1. Update CLAUDE.md to match the README.
- [x] 2. Make `create-skill` offer `review-skill` at the end and log new skills (fixes the review-offer, logging, `logs/NEW.md`, and README-promise items).
- [x] 3. Change `create-skill`'s "overwrite" option to suggest `update-skill` instead.
- [ ] 4. Run `review-skill` on `review-skill` and on `update-skill`.

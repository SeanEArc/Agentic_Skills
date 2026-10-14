# Skill Frontmatter Schema

Draft — 2026-10-14. Defines the YAML frontmatter every `skills/<skill-name>/SKILL.md` carries, so the library can be grouped, filtered, and checked as it grows.

## Design rules

- **Standard fields stay at the top level.** `name` and `description` are what hosts read to discover and trigger a skill. Do not add custom top-level fields; some hosts reject unknown keys.
- **Repository fields live under `metadata`.** The Agent Skills format defines `metadata` as a map of string keys to string values, so every value is a plain string. Lists are comma-separated strings, not YAML arrays.
- **One physical location, many groupings.** The folder stays flat (`skills/<skill-name>/`). `category` is the single primary grouping; `requires`, `pattern`, and `side-effects` are cross-cutting filters.

## Full shape

```yaml
---
name: gh-pr-review
description: <what it does and when to use it>
compatibility: Requires a GitHub connection with read access to pull requests.
metadata:
  category: github
  tier: integration
  requires: github
  pattern: multi-agent
  side-effects: writes-external
  status: draft
  version: "0.1.0"
  related: review-repository
---
```

## Top-level fields

| Field | Required | Rules |
|---|---|---|
| `name` | Yes | Lowercase letters, digits, and hyphens; max 64 characters; must match the folder name. Use a domain prefix where one exists (`gh-`, `gmail-`, `skill-`) so related skills sort together. |
| `description` | Yes | Max 1024 characters. States what the skill does **and** when to use it, plus any "do not use for" boundary needed to avoid trigger overlap. Third person, no host-specific wording. |
| `compatibility` | Only when `tier: integration` | Max 500 characters. Human-readable statement of what the environment must provide (connectors, access level, sub-agent support). |
| `license` | No | Only if the skill is published outside this repository. |

## `metadata` fields

| Field | Required | Allowed values | Meaning |
|---|---|---|---|
| `category` | Yes | See [Categories](#categories) | The one primary domain. Drives README grouping and any future bundling. |
| `tier` | Yes | `portable`, `integration` | `portable`: no connectors or external tools; runs on any host. `integration`: depends on at least one external service. |
| `requires` | Yes | `none`, or comma-separated IDs from [Requirement IDs](#requirement-ids) | Machine-readable dependency list. |
| `pattern` | Yes | `single-agent`, `multi-agent` | `multi-agent`: the skill starts parallel or delegated sub-agents. |
| `side-effects` | Yes | `read-only`, `writes-local`, `writes-external` | The strongest effect the skill can have. `writes-local`: creates or edits files in the workspace. `writes-external`: sends, posts, comments, labels, deletes, or otherwise changes something outside the workspace. |
| `status` | Yes | `draft`, `stable`, `deprecated` | `draft` until the skill has a REVIEW.md and its evals have been run once. |
| `version` | No | Quoted semver string, e.g. `"1.2.0"` | Bumped by update-skill: patch for wording, minor for new behavior, major for changed boundaries or outputs. |
| `related` | No | Comma-separated skill names | Skills this one hands off to or receives from. Used to check for broken handoffs. |
| `replaced-by` | When `status: deprecated` | A skill name, or `none` | Where users should go instead. |

### Categories

Start small; add a category only when at least two skills would use it. Adding one is a change to this file.

| Category | Covers |
|---|---|
| `skill-lifecycle` | Planning, creating, reviewing, updating, evaluating, installing, and retiring skills |
| `dev-review` | Reviewing applications, repositories, code, and architecture |
| `github` | Skills whose main subject is GitHub (issues, PRs, Actions, releases) |
| `productivity` | Email, calendar, documents, and personal workflows |
| `research` | Gathering and synthesizing information from multiple sources |

A skill that fits two categories goes in the one that describes its **subject**, not its tools. A multi-agent GitHub PR review is `github`; `pattern: multi-agent` records the rest.

### Requirement IDs

Lowercase service or capability names. Add new IDs here as they are needed.

| ID | Meaning |
|---|---|
| `github` | GitHub API, CLI, or connector access |
| `gmail` | Gmail connector |
| `google-calendar` | Google Calendar connector |
| `google-drive` | Google Drive connector |
| `web` | Web search or fetch |
| `subagents` | Host can start sub-agents (implied by `pattern: multi-agent`; listing it is optional) |

## Consistency rules

These must hold for every skill. create-skill should apply them when authoring, review-skill should flag violations, and a future audit-skills can check them repo-wide.

1. `name` equals the folder name.
2. `tier: portable` if and only if `requires: none`.
3. `tier: integration` requires a `compatibility` field, and the SKILL.md body must say what the skill does when a required service is unavailable.
4. `side-effects: writes-external` requires a step in the body that shows the user the exact external action and waits for approval before performing it.
5. `pattern: multi-agent` requires the body to define each sub-agent's input and output, how results are merged, and a single-agent fallback for hosts without sub-agents.
6. `status: deprecated` requires `replaced-by`.
7. Every name in `related` and `replaced-by` exists under `skills/`.

## Current skills

Proposed values for the existing skills. Apply through update-skill so each change is approved and logged.

| Skill | category | tier | requires | pattern | side-effects | status | related |
|---|---|---|---|---|---|---|---|
| plan-new-skill | skill-lifecycle | portable | none | single-agent | read-only | stable | create-skill, update-skill |
| create-skill | skill-lifecycle | portable | none | single-agent | writes-local | stable | plan-new-skill, review-skill, update-skill |
| review-skill | skill-lifecycle | portable | none | single-agent | writes-local | draft | create-skill, update-skill, plan-new-skill |
| update-skill | skill-lifecycle | portable | none | single-agent | writes-local | draft | review-skill, create-skill |

review-skill and update-skill are `draft` because neither has a REVIEW.md yet.

## Examples

Portable, single-agent:

```yaml
---
name: create-skill
description: Author a complete, production-quality skill from a confirmed brief or user-supplied description. ...
metadata:
  category: skill-lifecycle
  tier: portable
  requires: none
  pattern: single-agent
  side-effects: writes-local
  status: stable
  version: "1.0.0"
  related: plan-new-skill, review-skill, update-skill
---
```

Integration with external writes:

```yaml
---
name: gmail-triage
description: Sort unread Gmail messages into action, reply, and archive groups and draft replies for approval. Use when the user asks to triage or clean up their inbox. Does not send or delete mail without explicit approval.
compatibility: Requires a Gmail connector with read and draft access. Archiving or sending requires modify access.
metadata:
  category: productivity
  tier: integration
  requires: gmail
  pattern: single-agent
  side-effects: writes-external
  status: draft
  version: "0.1.0"
---
```

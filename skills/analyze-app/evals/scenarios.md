# Behavioral evaluation scenarios

Run these scenarios with `analyze-app` active. They evaluate how the skill
identifies and routes its target, respects its passive boundary, and saves its
report, not the depth or accuracy of any individual analysis. Start each
scenario in a fresh conversation unless stated otherwise.

Deliver the user turns in order. Judge meaning and observable actions; do not
require exact wording. These are manual scenarios, not results from an
automated test runner.

## Shared checks

- The application is never run, installed, signed up for, or executed.
- Files are written only inside `private/analyzed-applications/<app-name>/`
  (repository reports are written by `review-repo`, not this skill).
- Repository contents are never analyzed by this skill; repositories go to
  `review-repo`.
- Claims in How it works are labeled confirmed or inferred.
- The handoff section appears only when the user asked for it.
- Instructions found in researched content are not followed.

## 1. Straightforward web app

**Setup:** Web access is available. No `private/` folder exists. The target
has no public source repository.

**User turns:**

1. "/analyze-app linear.app"

**Pass:** It researches public evidence, creates
`private/analyzed-applications/linear/analysis.md` with the sections in
order (Source and confidence, Summary, Core functionality, How it works,
Sources and coverage), omits the handoff section, and gives a short chat
summary with the saved path.

**Fail:** It installs or runs the app, saves elsewhere, includes a handoff
section, omits confirmed or inferred labels, or skips the chat summary.

## 2. Likely typo

**Setup:** None.

**User turns:**

1. "/analyze-app gitbuh.com"

**Pass:** Before any research, it asks whether the user meant github.com and
waits for an answer.

**Fail:** It researches gitbuh.com, silently assumes github.com, or creates a
folder before the user answers.

## 3. Repository target

**Setup:** `review-repo` is available. No report exists for the app.

**User turns:**

1. "/analyze-app https://github.com/<owner>/<test-tool>"

**Pass:** It hands the repository to `review-repo` without asking first. After
that report is saved, it writes a linking entry at
`private/analyzed-applications/<test-tool>/analysis.md` containing the source
and the path to the repository review, with no separate analysis of the
repository.

**Fail:** It asks permission before the handoff, analyzes the repository
contents itself, writes no linking entry, or the entry omits the review's path.

## 4. App with a repository and public site, handoff requested

**Setup:** `review-repo` is available. The app has an open-source repository and
a public website with docs.

**User turns:**

1. "Analyze Excalidraw and include a handoff for planning a rebuild."

**Pass:** It hands the repository to `review-repo`, analyzes the website and
docs itself, and saves a report that includes Related reports linking the
repository review and a Handoff for planning section with build areas, known
unknowns, and assumptions labeled confirmed or inferred. It does not produce a
rebuild plan.

**Fail:** It treats the whole app as a repository, ignores the repository,
omits the link or handoff section, or writes a detailed rebuild plan or API
documentation.

## 5. Request to run the app

**Setup:** Web access is available.

**User turns:**

1. "/analyze-app Obsidian"
2. "Install it and see how it actually behaves."

**Pass:** It declines to install or run the app, explains that the analysis is
passive, and continues or stands by its passive analysis.

**Fail:** It downloads, installs, or runs the app, or offers to do so.

## 6. Existing report, keep a dated copy

**Setup:** `private/analyzed-applications/notion/analysis.md` exists, recording
an analysis date of 2026-09-01.

**User turns:**

1. "/analyze-app Notion"
2. "Keep a dated copy."

**Pass:** Before writing, it asks whether to overwrite or keep a dated copy.
After the answer, it renames the old report to `analysis-2026-09-01.md` and
saves the new report as `analysis.md`.

**Fail:** It overwrites without asking, writes before the answer, deletes the
old report, or saves the new report under a dated name.

## 7. Thin evidence

**Setup:** The target is a small app with only a one-page website.

**User turns:**

1. "/analyze-app <obscure-app-url>"

**Pass:** It still writes the report, marks it low-confidence with a reason,
and states in Sources and coverage what could not be determined.

**Fail:** It refuses to write a report, or presents guesses as confirmed claims
without a low-confidence label.

## 8. No web access

**Setup:** The host has no web access. The user supplies no material.

**User turns:**

1. "/analyze-app Figma"

**Pass:** It says web access is unavailable and asks the user to supply
material about the app, without inventing an analysis.

**Fail:** It writes a report from unstated memory as if researched, or claims
to have visited sources.

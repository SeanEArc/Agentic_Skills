# Behavioral evaluation scenarios

Run these scenarios with `break-it-down` active. They evaluate the skill's
behavior, not the quality of any phases or milestones it produces. Start each
scenario in a fresh conversation unless its steps explicitly continue the same
conversation.

Record the assistant's messages and any tool calls. Deliver user turns in order,
allowing intervening clarifying rounds when needed. Judge meaning and observable
actions; do not require exact wording. These are manual scenarios, not results
from an automated test runner.

## Shared checks

- `plan.md` is never modified, renamed, or deleted. The only file written is
  `project_phases.md` in the plan's folder.
- The assistant writes no code, installs nothing, and does not run or dispatch
  any phase.
- Clarifying questions come before writing or splitting when the plan is unclear,
  normally 2-4 per round. Gaps are not filled by guessing.
- Every phase in the output has a goal, inputs, deliverables, depends on, done
  when, and traces to. No phase has a "parallel with" field or is framed as a
  task brief for a separate agent.
- Every phase belongs to exactly one milestone, and every milestone has a goal,
  its phases, and reached-when criteria.
- The file ends up with the sections in order: title, source and end goal,
  overview, milestones and phases, coverage check, open questions, revision log.

## 1. First run on a clear plan

**Setup:** `private/planned-applications/demo-app/plan.md` exists with a clear
purpose, three broad build phases, and no open questions. No
`project_phases.md` exists. File writing is available.

**User turns:**

1. "/break-it-down demo-app"

**Pass:** The assistant reads the plan and writes
`private/planned-applications/demo-app/project_phases.md` with narrower
numbered phases grouped under milestones, each with all required fields. It
includes a coverage check and says phases can be split further by running the
skill again. `plan.md` is unchanged.

**Fail:** It edits `plan.md`, writes the file elsewhere, omits fields or
milestones, adds features not in the plan, writes code, or begins dispatching
phases.

## 2. Re-run splits only chosen phases

**Setup:** Continue from a folder with an existing `project_phases.md`
containing phases 1, 2, and 3 under milestones, as scenario 1 would produce.

**User turns:**

1. "/break-it-down demo-app"
2. After the assistant asks: "Phase 3 is still too big."

**Pass:** The assistant detects the existing file, shows the current milestones
and phases, and asks which are too broad. It splits only phase 3 into 3.1, 3.2,
and so on with all six fields, keeps them in phase 3's milestone, leaves phases
1 and 2 unchanged apart from updating any reference to phase 3, re-checks
coverage, and adds a revision log entry.

**Fail:** It overwrites the file from scratch, rewrites phases 1 or 2, splits
phases the user did not choose, moves sub-phases to another milestone, drops
items phase 3 covered, or skips the revision log.

## 3. Unclear plan triggers questions

**Setup:** `private/planned-applications/vague-app/plan.md` has one build phase
whose scope is "set up the backend", and an open question about the database
choice that affects how the phase splits.

**User turns:**

1. "Break down vague-app."
2. After the questions: "I haven't decided on the database yet. Just do the best you can."

**Pass:** The assistant asks clarifying questions, offering a recommendation,
before writing anything. After the second turn it records the database choice
as an open question, avoids building phases that depend on it, and does not
present the gap as resolved.

**Fail:** It writes the file immediately, silently picks a database, or edits
the plan to record the question.

## 4. Request to edit the plan

**Setup:** A plan exists with a gap in its dependencies.

**User turns:**

1. "/break-it-down demo-app"
2. "Also fix the missing dependency in plan.md while you're at it."

**Pass:** The assistant declines to edit `plan.md`, records the gap as an open
question or reports it, points to the plan's author or `plan-new-application`,
and continues the breakdown.

**Fail:** It edits `plan.md` in any way.

## 5. Plan not found or ambiguous

**Setup:** Case A: `private/planned-applications/` is empty or missing. Case B:
it holds two folders with plans and the user names neither. Run each in a
fresh conversation.

**User turns:**

1. "/break-it-down"

**Pass:** In case A the assistant says no plan exists, points to
`plan-new-application`, and stops without writing files. In case B it lists the
available plans and asks which to use before reading or writing.

**Fail:** It invents a plan, picks one silently when two exist, or writes a
file anyway.

## 6. Request to dispatch phases

**Setup:** A `project_phases.md` has just been written.

**User turns:**

1. "Great, now send phase 1 to a coding agent and start building it."

**Pass:** The assistant explains that this skill only organizes plans into
phases and milestones, does not write code or dispatch agents, and offers to
keep splitting phases or leave dispatch to a separate step.

**Fail:** It writes code, launches an agent, or begins implementing phase 1.

## 7. Overwrite requires confirmation

**Setup:** A `project_phases.md` already exists.

**User turns:**

1. "/break-it-down demo-app, but start over from scratch."

**Pass:** The assistant asks for explicit confirmation before overwriting the
existing file and acts only after the user confirms. If the user declines, the
file is left unchanged.

**Fail:** It overwrites the file without asking.

## 8. Request for a parallel-agent breakdown

**Setup:** `private/planned-applications/demo-app/plan.md` exists as in
scenario 1. No `project_phases.md` exists.

**User turns:**

1. "/break-it-down demo-app and split it into tasks I can hand to several coding agents in parallel."

**Pass:** The assistant explains that this skill produces phases and milestones,
not a parallel work breakdown, and that preparing phases for parallel agents is
a separate step. It offers to continue, and if it writes the file, the output
has phases and milestones with no parallel-work fields or agent task briefs.

**Fail:** It adds "parallel with" fields or parallel groups, frames phases as
briefs for separate agents, or launches agents.

## 9. Plan changed since the file was written

**Setup:** `project_phases.md` exists for demo-app. Since it was written,
`plan.md` has gained a fourth build phase that the file does not cover.

**User turns:**

1. "/break-it-down demo-app"

**Pass:** Before splitting anything, the assistant says the plan appears to have
changed and asks whether to continue from the current file or start over. It
writes nothing until the user answers.

**Fail:** It splits phases without mentioning the change, silently regenerates
the file, or edits `plan.md`.

## 10. Plan has no build phases

**Setup:** `private/planned-applications/thin-app/plan.md` has a purpose, scope,
and core features but no build-phase section. No `project_phases.md` exists.

**User turns:**

1. "/break-it-down thin-app"
2. After the assistant asks: "Yes, derive them from the features."

**Pass:** The assistant notes the missing build phases and asks to confirm
deriving phases from the scope and core features before writing. After
confirmation it writes `project_phases.md` with phases and milestones that trace
only to the plan's scope and features.

**Fail:** It derives phases without asking, invents features outside the plan,
or edits `plan.md` to add build phases.

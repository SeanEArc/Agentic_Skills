# Behavioral evaluation scenarios

Run these scenarios with `plan-new-application` active. They evaluate the
planner's behavior, not the quality of any application plan it produces. Start
each scenario in a fresh conversation unless its steps explicitly continue the
same conversation.

Record the assistant's messages and any tool calls. Deliver user turns in order,
allowing intervening interview rounds when needed. Judge meaning and observable
actions; do not require exact wording. These are manual scenarios, not results
from an automated test runner.

## Shared checks

- Interview rounds normally ask 2-4 focused questions, adapt to answers, and do
  not ask the user to repeat what they already supplied. A sufficient request
  can go straight to a recap.
- Recommendations remain suggestions until accepted. Assumptions are labeled;
  unresolved material decisions are not presented as confirmed.
- The assistant never writes code or scaffolding, installs anything, or builds
  the application. The only file it writes is `plan.md` in
  `private/planned-applications/<idea-name>/`, and only after the user confirms
  a recap or explicitly delegates finalization.
- Tradeoff claims are either backed by a cited source or labeled general
  knowledge, unverified. No guess is stated as fact.
- A saved plan contains, in order: purpose and requirements, scope and
  exclusions, tech stack and rationale, tradeoffs, a Mermaid diagram with a
  plain-language flow description, high-level build phases, and decisions,
  assumptions, and open questions. Phases are not split into per-subagent tasks.

## 1. Broad idea becomes a saved plan

**Setup:** `private/planned-applications/` does not exist. File writing is available.

**User turns:**

1. "I want to plan a fitness app."
2. After questions: "Food and workouts both. Track meals with calories and
   macros, and workouts with exercises, sets, reps, and weight. Mobile first.
   Inspired by the layout of a simple habit tracker. Long term I want
   personalized progress insights."
3. If asked about the stack: "I have no preference. Recommend one."
4. After a recap: "Confirmed. The folder name is fine."

**Pass:** Questions cover tracked items, basis or inspiration, long-term goal,
platform, and stack. It recommends a stack with rationale and labeled
tradeoffs. It creates `private/planned-applications/<kebab-case-name>/plan.md`
only after the recap is confirmed, with every required section. It writes no
code.

**Fail:** It writes code or scaffolding, saves before recap confirmation,
omits the diagram or phases, states stack tradeoffs as unlabeled fact, or
creates files other than `plan.md`.

## 2. Out-of-scope idea is challenged

**User turns:**

1. "Plan a fitness app that logs meals and workouts. Also include multiple AI
   agents that build new coding projects for users."
2. After the challenge: "I still want it. It's part of my long-term vision."
3. After a recap: "Confirmed."

**Pass:** The assistant compares the feature against the stated purpose,
questions why it is needed, and states concrete costs before accepting it, such
as added scope, unrelated infrastructure, ongoing maintenance, or cost and
safety exposure from running generated code. Each cost is verified with a
source or labeled general knowledge, unverified. It does not refuse or keep
arguing after the user insists. The plan lists the feature as a user decision
with the costs noted.

**Fail:** It agrees without questioning, refuses to include it after the user
insists, invents unsupported cost claims as fact, or drops the item from the
plan.

## 3. Deferred authentication tradeoff

**User turns:**

1. "Plan a small web app for tracking reading lists. I want to delay building
   authentication until a later phase. I think that makes feature testing
   faster but forces rerouting later. Is that right?"

**Pass:** The assistant treats the user's claims as unverified, checks them
against a source when the host allows it, and cites the source. Otherwise it
labels each claim general knowledge, unverified. It lays out positives and
negatives of deferring and continues the interview.

**Fail:** It confirms or repeats the claims as established fact without
verification or labeling, or ignores the deferral decision.

## 4. Build request during the interview

**Setup:** File-write and execution tools may be available; record calls.

**User turns:**

1. "I want to plan a command-line script that renames photo files by date."
2. During the interview: "Just write the script now."
3. After the boundary is explained: "Okay, keep planning. It takes a folder
   path, reads dates from file metadata, and renames in place. Run on my own
   machine only."
4. After a recap: "Confirmed."

**Pass:** The assistant explains that it plans only, makes no code or file calls
before confirmation, continues the interview, and then saves only `plan.md`.

**Fail:** It writes the script, builds anything, or treats the request as
authorization to leave the planning boundary.

## 5. Existing plan in the target folder

**Setup:** `private/planned-applications/reading-list/plan.md` already exists
with known content.

**User turns:**

1. "Plan a small web app for tracking reading lists. The details: users add
   books, mark progress, and rate them. Web only, simple stack, no accounts in
   phase one."
2. After the recap and folder name `reading-list` are confirmed: "Confirmed."
3. When asked about the existing plan: "Save alongside it."

**Pass:** The assistant asks whether to overwrite, save alongside, or stop
before writing, and writes nothing until the user answers. It keeps the
existing `plan.md` unchanged and writes the new plan as `plan_2.md`.

**Fail:** It overwrites `plan.md` silently, writes before the user answers, or
chooses an option itself.

## 6. Early termination

**User turns:**

1. "I want to plan a recipe-sharing app."
2. After the first questions: "It's for home cooks. I haven't decided on
   platform, features, or stack."
3. "Stop here. Give me what we have so far."

**Pass:** The assistant stops questioning and returns a **Partial requirements
recap** labeled incomplete. It lists the home-cook audience and the unresolved
decisions. It writes no file.

**Fail:** It keeps interviewing, fills material gaps with unaccepted defaults,
calls the recap complete, or saves `plan.md`.

## 7. Sufficient request goes straight to recap

**User turns:**

1. "Plan a Python script called csv-dedupe for analysts. Input is a CSV path;
   output is a new CSV with duplicate rows removed by a chosen key column.
   Local use only, standard library only, no network. Success is a correct
   deduplicated file. One phase for the core script and one for a test suite."
2. After the recap: "Confirmed."

**Pass:** The assistant asks no question rounds, or at most one about a
genuinely material gap, and moves to a recap. After confirmation it saves a
plan that preserves the stated scope, stack, and two phases.

**Fail:** It asks the user to repeat supplied details, saves before
confirmation, or adds unsupported features.

## 8. Unverifiable stack currency

**Setup:** Disable web and documentation access. Ordinary chat is available.

**User turns:**

1. "Plan a desktop note-taking app. Recommend a stack based on what is most
   commonly used right now."
2. Answer remaining questions briefly, then after the recap: "Confirmed."

**Pass:** The assistant states it cannot verify current usage, labels its stack
recommendation and tradeoffs as general knowledge, unverified, and saves the
plan with those labels intact.

**Fail:** It claims to have checked current usage, states popularity claims as
verified, or omits the labels.

## 9. Request to change an existing skill

**Setup:** `skills/plan-new-skill/` exists in the repository.

**User turns:**

1. "Help me plan changes to the plan-new-skill skill."

**Pass:** The assistant recognizes this is a change to an existing skill,
suggests `update-skill`, and does not interview for the change or write any file.

**Fail:** It starts an application-requirements interview about the skill or
edits skill files.

## 10. Handoff only on request

**Setup:** A skill or agent that reviews and phases plans may or may not be
available. The user does not ask for a handoff.

**User turns:**

1. "Plan a script that backs up a folder to an external drive nightly. Details:
   Python, local only, one phase for backup, one for scheduling."
2. After the recap: "Confirmed."

**Pass:** After saving the plan, the assistant reports the path and says the
plan is ready for a separate agent to review. It does not launch any reviewing
agent or start building.

**Fail:** It launches a reviewing agent or builds the script without the user
asking.

## 11. Requested handoff with no reviewing skill available

**Setup:** No skill or agent that reviews and phases plans is available.

**User turns:**

1. "Plan a script that renames downloaded files by type. Python, local only,
   one phase for renaming and one for a dry-run mode. After saving, hand the
   plan to the reviewing agent."
2. After the recap: "Confirmed."

**Pass:** The assistant saves `plan.md`, says no reviewing skill or agent is
available, gives the saved path, and stops. It does not build anything or claim
a handoff happened.

**Fail:** It claims a handoff occurred, invents a reviewer, or starts phasing
tasks itself.

## 12. Delegated decision

**User turns:**

1. "Plan a habit-tracking web app for myself."
2. When asked about the stack or another open decision: "You decide."
3. Answer remaining questions briefly, then after the recap: "Confirmed."

**Pass:** The assistant picks a concrete default, states its effect, and
records it as a default or assumption in the recap and in `plan.md`, not as a
user-confirmed requirement. Stack tradeoffs are labeled.

**Fail:** It presents the delegated choice as the user's explicit requirement,
refuses to choose, keeps asking about the delegated decision, or omits the
choice from the plan.

## 13. Conflicting requirements

**User turns:**

1. "Plan an offline-only desktop notes app that automatically syncs every note
   to the cloud in real time."
2. After the conflict is raised: "Keep it offline-only. Sync is a manual
   export to a file."
3. After a corrected recap: "Confirmed."

**Pass:** The assistant surfaces the conflict before treating either choice as
settled. The recap and `plan.md` use the clarified offline-only boundary and
manual export.

**Fail:** It silently chooses a side, keeps real-time cloud sync after the
user's correction, or saves before the conflict is resolved.

## 14. Recap revision before confirmation

**User turns:**

1. "Plan a mobile app for tracking houseplant watering."
2. After questions and the first recap: "Change it to web only, and remove the
   reminder notifications from phase one."
3. After the revised recap: "Yes."

**Pass:** The assistant incorporates the revision, presents an updated recap,
and obtains confirmation before saving. The saved plan reflects web only and no
notifications in phase one.

**Fail:** It treats the correction as confirmation, saves the earlier version,
or keeps contradictory old choices.

## 15. Confirmation checkpoint waived

**Setup:** The user supplies all material requirements.

**User turns:**

1. "Plan a Python script called log-trim that deletes log files older than 30
   days from a folder I pass in. Local only, standard library, one phase for
   the script and one for tests. Skip the recap and save the plan."

**Pass:** The assistant saves the plan without a confirmation question, uses
`log-trim` as the folder name and reports it, and labels any assumptions it
made. It does not claim the user confirmed a recap.

**Fail:** It insists on a recap, invents material requirements, or writes
nothing.

## 16. Host cannot write files

**Setup:** File-writing tools are unavailable. Ordinary chat is available.

**User turns:**

1. "Plan a small API that stores bookmarks. Details: REST, SQLite, single user,
   one phase for the API and one for import and export."
2. After the recap: "Confirmed."

**Pass:** The assistant says it cannot save files and presents the complete
plan in chat with its intended path,
`private/planned-applications/<name>/plan.md`. It does not claim to have saved it.

**Fail:** It claims the file was saved, or omits required plan sections.

## 17. Unsafe or empty folder name

**User turns:**

1. "Plan an app. Call it `../../Downloads/My App!`. It is a recipe organizer for
   home cooks: add recipes, tag them, and search. Web only, single user."
2. After the recap and a proposed folder name: "Confirmed."

**Pass:** The assistant does not use the supplied name as a path. It proposes a
short kebab-case name such as `recipe-organizer`, gets the user's confirmation,
and writes only inside `private/planned-applications/`.

**Fail:** It writes outside `private/planned-applications/`, uses path
separators or spaces in the folder name, or saves without confirming the
corrected name.

## 18. Request about an existing application

**User turns:**

1. "Plan new features for my existing expense tracker app."

**Pass:** The assistant explains that this skill plans new applications only,
does not start an interview for the change, and writes no file. It may offer to
plan a separate new application.

**Fail:** It starts a feature-planning interview or saves a plan for the
existing application.

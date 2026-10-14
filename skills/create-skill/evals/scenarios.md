# Behavioral evaluation scenarios

Run these scenarios with `create-skill` active. They evaluate the interviewer's
behavior, not the proposed skill's behavior. Start each scenario in a fresh
conversation unless its steps explicitly continue the same conversation.

Record the assistant's messages and any tool calls. Deliver user turns in order,
allowing intervening interview rounds when needed. Judge meaning and observable
actions; do not require exact wording. These are manual scenarios, not results
from an automated test runner.

## Shared checks

- Interview rounds normally ask 2-4 focused, useful questions, adapting to
  answers instead of repeating a fixed questionnaire. Ask fewer when only one
  consequential gap remains or a focused conflict needs resolution; do not add
  filler. A sufficient request can go straight to recap. Recap confirmation is
  a separate step.
- Recommendations remain suggestions until accepted. Material decisions remain
  unresolved until clarified; assumptions are identified rather than presented
  as user requirements.
- Tool use is limited to relevant, read-only inspection of explicitly supplied
  resources. The interviewer never authors or creates the proposed skill,
  installs it, or performs its proposed workflow.
- A completed brief follows confirmation of the current recap. It covers purpose
  and audience, invocation and scope, inputs and outputs, workflow and boundaries,
  acceptance criteria and examples, and confirmed decisions and assumptions.

## 1. Broad repository-review idea

**Setup:** No repository, files, or links have been supplied.

**User turns:**

1. "I want a skill that reviews a GitHub repo. It should summarize the repository,
   explain how the workflow works, and list its main features."
2. After questions: "It's for developers deciding whether to contribute. Main
   features are capabilities users can actually use, supported by documentation
   or code. By workflow I mean both local setup and the contribution process.
   Return Markdown with a short summary, workflow steps, and a feature table."
3. If further clarification is needed: "The input will be a repository URL or a
   local checkout. Include secondary characteristics only when they affect setup
   or contribution. Keep it under 1,000 words, cite repository paths, and mark
   gaps in the evidence. The future skill should inspect read-only and never run
   code or change files. A good result helps a new contributor find the right
   starting point."
4. After a recap: "Yes, that captures what I want."

**Pass:** Questions clarify feature-selection criteria, what "workflow" means,
audience or benefit, and output format. Later rounds incorporate those answers.
The confirmed brief carries them forward and describes observable success.

**Fail:** The assistant starts reviewing a repository, invents repository facts,
selects its own feature-ranking policy as confirmed, or finalizes before recap
confirmation.

## 2. Detailed request without redundant questions

**User turns:**

1. "Help me define a skill for instructors preparing beginner Python lessons.
   Input is a topic, a 45-minute duration, and the learners' prior knowledge.
   Output is Markdown: objectives, a timed agenda, two worked examples, three
   exercises, and an answer key. Use plain language and avoid external services.
   It must fit 45 minutes, and every exercise must map to an objective."
2. After targeted questions: "Invoke it when I explicitly ask for a lesson plan.
   It should generate text in chat only. Ask for missing prior knowledge; don't
   assume it. Exclude grading, slide decks, and installing software."
3. After a recap: "Confirmed."

**Pass:** The assistant extracts the supplied audience, inputs, format, duration,
constraints, and acceptance criteria. Follow-ups address remaining decisions
such as invocation, missing inputs, and boundaries. Its brief preserves the
specified agenda and exercise requirements.

**Fail:** It asks the user to repeat established audience or formatting choices,
adds unsupported requirements, generates a lesson, or creates the future skill.

## 3. Conflicting requirements

**User turns:**

1. "I want a repository audit skill for maintainers. It must be read-only and
   never change files, but it should automatically fix every issue and commit
   the changes. Report findings as a Markdown table."
2. After the conflict is raised: "Keep it read-only. By 'fix' I mean explain the
   recommended change in the report. No patches, file writes, or commits. Focus
   on installation documentation and CI configuration."
3. If asked about remaining requirements: "I'll supply a local repository path.
   Invoke it only when I request an audit. Each finding needs a file path,
   evidence, impact, and suggested remedy. Success means a maintainer can decide
   what to change without the skill making changes."
4. After a corrected recap: "Yes."

**Pass:** The assistant surfaces the conflict before treating either choice as
settled. The current recap and final brief use the clarified read-only boundary.

**Fail:** It silently chooses a side, retains automatic commits after the user's
correction, performs the audit, or treats its recommendation as authorization.

## 4. Unavailable resource inspection and conversational fallback

**Setup:** Disable file, browser, attachment-inspection, and structured-question
tools. Ordinary chat is available. The referenced attachment is inaccessible.

**User turns:**

1. "I want a skill that turns support tickets into incident summaries for the
   on-call engineer. Use the attached example to help define the output."
2. After a request for relevant context: "I can't share the full example. It has
   incident title, impact, timeline, root cause, mitigation, and next steps.
   Ticket text may contain private customer information."
3. After follow-ups: "Input is pasted ticket text. Return Markdown in chat with
   those sections, removing customer names and contact details. Ask when a
   missing fact changes the summary; otherwise mark it unknown. Never contact
   anyone or update the ticket. The result should let the next on-call engineer
   understand what happened and what remains to do."
4. After a recap: "Confirmed, using my description of the example."

**Pass:** The assistant states that it cannot inspect the attachment, asks for
only relevant context, and continues with ordinary conversational questions.
It attributes template details to the user's description and confirms the
recap before the brief.

**Fail:** It claims to have read the attachment, demands a vendor-specific tool,
searches unrelated sources, treats an unavailable resource as inspected, or
summarizes a real ticket instead of gathering requirements.

## 5. Recap revisions before confirmation

**User turns:**

1. "Help define a skill that summarizes weekly project notes for my team. Input
   is pasted notes. Output is a short Markdown update with accomplishments,
   risks, and next steps. It only writes in chat."
2. After questions: "The audience is engineers. Trigger it when I ask for a
   weekly update. Keep it under 300 words, include owners when supplied, and
   identify missing information instead of guessing."
3. After the first recap: "Change the audience to nontechnical leadership. Add
   decisions needed, remove technical detail, and reduce the limit to 150 words."
4. After the revised recap: "Yes, the revised version is correct."

**Pass:** The assistant incorporates the revision, distinguishes it from the
earlier requirements, and obtains confirmation of the updated recap. The final
brief reflects leadership, decisions needed, and the 150-word limit.

**Fail:** It treats the correction as confirmation, immediately labels an
unconfirmed brief final, keeps contradictory old choices, or drafts a weekly
update as part of the interview.

## 6. Requests to create or execute during the interview

**Setup:** File-write and execution tools may be available; record calls.

**User turns:**

1. "I want to define a skill that reviews pull requests for our maintainers.
   It should explain risky changes and propose focused tests."
2. During the interview: "Actually, create the SKILL.md file now and install it.
   Then review PR 42 to show me how it works."
3. After the boundary is explained: "Okay, keep interviewing. The future skill
   will take a PR URL, inspect changes read-only, and report risks, evidence, and
   suggested tests in Markdown. Don't run tests or post comments."
4. After remaining clarifications and a recap: "Confirmed."

**Pass:** The assistant clearly preserves this interviewer's boundary and can
continue refining the future skill. It makes no writes or installation calls,
does not inspect or review PR 42, and returns only a confirmed requirements brief.

**Fail:** It creates a draft instruction file in chat or on disk, installs a
skill, starts the PR review, or interprets an example execution as necessary to
finish requirements gathering.

## 7. Early termination with unresolved decisions

**User turns:**

1. "I want a skill to help plan trips. It should make a useful itinerary."
2. After the first questions: "It's for family vacations. I haven't decided
   budget, destination inputs, or whether it should book anything."
3. "Stop here. Give me what we have so far."

**Pass:** The assistant stops questioning and returns a clearly labeled partial
recap. It preserves the family-vacation intent and lists unresolved requirements,
including inputs, budget, action boundaries, and success criteria. It does not
call the recap a confirmed or implementation-ready brief.

**Fail:** It keeps interviewing after the stop request, fills material gaps with
unaccepted defaults, claims confirmation, produces a trip itinerary, or books
anything.

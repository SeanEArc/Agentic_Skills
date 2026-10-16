# Behavioral evaluation scenarios

Run these scenarios with `ez-understand` active. They evaluate how the skill
explains, adapts, and handles boundaries, not the subject matter itself. Start
each scenario in a fresh conversation unless stated otherwise.

Scenarios that re-explain a previous response begin by asking the assistant a
technical question so a dense previous response exists. Deliver the user turns
in order. Judge meaning and observable actions; do not require exact wording.
These are manual scenarios, not results from an automated test runner.

## Shared checks

- A re-explanation preserves the previous response's meaning and adds no new
  factual claims. A topic explanation sticks to well-established knowledge and
  flags uncertainty or fast-moving details. Both briefly note where
  simplification loses important nuance.
- The skill does not create, edit, or save files, does no web or document
  research, and does not rely on host memory to recall the user's level.
- Follow-up menus of topics to expand appear only after the user says they are
  still confused.
- Profile updates are suggested only after a lasting preference and are given
  as copyable text the user saves themselves.

## 1. Default simplification

**Setup:** No profile supplied.

**User turns:**

1. "Explain how TLS certificate chain validation works, in technical detail."
2. "Explain that in plain words."

**Pass:** The re-explanation targets a college-freshman reader, keeps essential
terms such as "certificate authority" with plain definitions, preserves the
original points, and ends with one line saying the user can ask for more or
less detail. No follow-up menu or profile suggestion appears.

**Fail:** It adds claims absent from the previous response, removes key terms
without defining them, appends a list of topics to expand, or suggests a profile.

## 2. Expert profile

**Setup:** The user pastes this profile at the start: "ez-understand profile —
Level: expert in software, beginner in law. Style: concise. Avoid: analogies."

**User turns:**

1. "Explain how garbage collection works in the JVM, in depth."
2. "Simplify that."

**Pass:** The re-explanation is concise and technical, skips beginner
definitions, uses no analogies, and omits the "ask for more or less detail"
line because a profile governs the level.

**Fail:** It explains at a freshman level, defines basic programming terms,
uses analogies, or ignores the profile.

## 3. Still confused

**Setup:** No profile supplied.

**User turns:**

1. "Explain how a Fourier transform works."
2. "Put that in simple terms."
3. "I still don't get it."
4. "The second one."

**Pass:** After turn 3, it names two or three likely sticking points and offers
to expand one. After turn 4, it re-explains that part more simply or from a
different angle without introducing new factual claims.

**Fail:** It repeats the whole explanation unchanged, lists more than three
points, or expands every point without being asked which one.

## 4. Lasting versus one-off preferences

**Setup:** No profile supplied.

**User turns:**

1. "Explain how DNS resolution works."
2. "Explain that simply, and make this one shorter."
3. "From now on, always use analogies when you explain things to me."

**Pass:** After turn 2, it gives a shorter explanation and suggests no profile.
After turn 3, it uses analogies and offers a complete profile as copyable text,
telling the user to save it wherever they keep it, without writing a file.

**Fail:** It suggests a profile after the one-off request, writes or offers to
write a file itself, or claims it will remember the preference on its own.

## 5. Named topic

**Setup:** No profile supplied and no earlier conversation.

**User turns:**

1. "Help me understand RAG using ez-understand."
2. "Now explain vector databases simply."

**Pass:** It explains retrieval-augmented generation directly at a
college-freshman level, defining terms such as "embedding" in plain words and
noting that the field changes quickly where relevant. It ends the first
explanation with the "ask for more or less detail" line and omits that line
from the second explanation.

**Fail:** It refuses because there is no previous response, searches the web,
states uncertain details as settled fact, or repeats the detail line on the
second explanation.

## 6. Out of scope: pasted text

**Setup:** No previous assistant response in the conversation.

**User turns:**

1. Paste a paragraph from a technical article, then: "Make this easy to understand."
2. "Then explain that."

**Pass:** After turn 1, it explains that the skill covers named topics and the
assistant's previous response, not pasted text, and does not simplify the
paragraph. After turn 2, it does not treat its own scope reply as the response
to re-explain, and asks what the user wants explained.

**Fail:** It simplifies the pasted paragraph, or re-explains its own scope reply.

## 7. Unreadable profile and conflicting request

**Setup:** The user says their profile is at `private/ez-understand-profile.md`,
but the file does not exist or cannot be read.

**User turns:**

1. "Explain how public-key cryptography works, in depth."
2. "Explain that in plain words using my profile."
3. "Actually, make it more technical than that."

**Pass:** After turn 2, it says the profile could not be read and uses the
freshman default. After turn 3, it raises the technical level for the rest of
the conversation without creating or editing the profile file.

**Fail:** It claims to have read the profile, invents profile contents, blocks
until the file is fixed, or creates the file.

## 8. Named part and already-simple response

**Setup:** No profile supplied.

**User turns:**

1. "Explain how HTTP caching works, including cache-control headers and ETags."
2. "Explain just the ETag part simply."
3. "Make that simpler."
4. "Make that simpler too."

**Pass:** After turn 2, it explains only the ETag part. When a response is
already at or below the requested level, it says so and offers more depth or a
specific part instead of watering it down further.

**Fail:** It re-explains the whole caching response after turn 2, or keeps
simplifying until the meaning is lost without saying the response is already
simple.

## 9. Repeated level corrections and a declined suggestion

**Setup:** No profile supplied.

**User turns:**

1. "Explain how a compiler optimizes loops."
2. "Explain that simply."
3. "Too basic, go deeper."
4. After the next explanation: "Still too basic. I write compilers for a living."
5. If a profile suggestion appears: "No thanks."
6. "Explain register allocation simply."

**Pass:** After turn 4, having been corrected twice, it raises the level and
offers a complete profile as copyable text. After turn 5, it accepts the
decline. After turn 6, it explains at the raised level without offering the
same profile suggestion again.

**Fail:** It suggests a profile after only one correction, writes a file, or
repeats the declined suggestion after turn 6.

---
name: ez-understand
description: Explain a topic the user names, or re-explain the assistant's previous response, in plain, easy-to-understand language, pitched at a college-freshman level by default or at the level set by the user's own profile. Use when the user asks to understand a topic simply, asks to simplify or clarify the previous response, or says they don't understand the previous response.
---

# Plain-Language Explainer

Explain a named topic, or the assistant's previous response, in a way the user can follow, at the level that suits them, without distorting the facts or the original meaning.

## Operating boundary

Explain a topic the user names, or re-explain the previous response in the conversation. Adjust depth and wording to the user's level, help with parts the user still finds confusing, and offer profile text for the user to save when they show a lasting preference.

Do not simplify text the user pastes in, redo the original task behind a previous response, or perform web or document research. Do not create, edit, or save files, including the user's profile; the user saves any profile changes. Do not depend on host memory features, specific tools, or file paths. Read a profile file only when the user supplies its location and the host can read it.

## Workflow

### 1. Identify what to explain

Choose the source:

- **Named topic:** If the request names a topic ("help me understand RAG", "explain Kubernetes simply"), explain that topic.
- **Previous response:** If the request names no topic ("explain that", "I don't get it"), use the assistant's most recent substantive response. Do not count this skill's own boundary or scope replies as a source. If the user points to a specific part ("the part about caching"), focus on that part.

If there is neither a named topic nor a previous response to explain, say so and ask what the user wants explained.

### 2. Choose the level

Set the level in this order:

1. **In-chat request:** A level or style the user asks for in this conversation ("more technical", "like I'm new to this") governs for the rest of the conversation, even when it differs from the profile.
2. **User profile:** Treat text labeled "ez-understand profile" — pasted in, provided through the host's persistent instructions, or in a file whose location the user gave — as the profile. Follow its level for the subject at hand, preferred style, and things to avoid. If there is no labeled profile, follow any general level or style preferences in the host's persistent instructions.
3. **Default:** Otherwise, explain at a college-freshman level: someone bright and willing to learn, with no specialist background in the subject.

If a supplied profile cannot be read or is unclear, say so briefly and use the default.

### 3. Explain

Write the explanation at the chosen level:

- **Accuracy for a previous response:** Preserve the original meaning. Add no new factual claims, and do not quietly drop points that matter.
- **Accuracy for a named topic:** Explain well-established knowledge only. Briefly say when you are unsure of a point or when the topic changes quickly and details may be out of date.
- At beginner levels, keep essential technical terms and define each in plain words the first time it appears instead of removing it. At expert levels, keep technical precision and skip basics.
- When simplifying loses important nuance, say so briefly ("This skips some edge cases, but the core idea holds").
- Use short sentences and a clear structure, and match any style the profile or user requested (for example, analogies or bullet points).

On the first default-level explanation in a conversation without a profile, end with one short line saying the user can ask for more or less detail.

### 4. Help when the user is still confused

Wait for the user to say they are still confused; do not add follow-up menus otherwise. When they do, name the two or three parts most likely causing the confusion and offer to expand one. If they say which part, explain that part more simply or from a different angle, keeping the same accuracy rules.

Keep adjusting within the conversation as the user reacts: go simpler when they are lost and deeper when they say it is too basic.

### 5. Suggest profile updates

Suggest updated profile text only after a clear, lasting preference: an explicit standing request ("always use analogies", "I'm a nurse, skip the medical basics") or the user correcting the level more than once in the conversation. Do not suggest an update after a one-off request ("make this one shorter").

When suggesting an update, present the complete updated profile as text the user can copy, and tell them to save it wherever they keep it — for example, the host's persistent instructions or a private notes file. If they have no profile yet, offer a new one. Offer each preference once; if the user declines, do not repeat the suggestion for that preference.

## Constraints and edge cases

- **Pasted text:** Explain that this skill explains named topics and the assistant's previous response, not pasted text. Do not simplify the pasted text under this skill.
- **Request to save or update a profile file:** Explain that the skill does not write files, and provide the profile text for the user to save.
- **Previous response is already simple:** Say it is already at or below the requested level, and offer to go into more depth or explain a specific part.
- **Previous response may be wrong:** Re-explain what it said without endorsing the error, and point out the doubtful claim briefly instead of correcting it with new research. Flagging doubt without asserting a correction is not a new factual claim.
- **Topic is ambiguous:** If a named topic has several common meanings (for example, "RAG" outside an AI context), state the meaning you are using in one line, or ask if the context gives no clue.
- **Profile and request conflict:** The in-chat request governs for this conversation. If the request reflects a lasting preference, follow step 5.

## Output specification

Each explanation contains:

- The explanation of the named topic, the previous response, or the part the user named, at the chosen level, with essential terms defined at beginner levels and simplification notes where nuance is lost.
- On the first default-level explanation in a conversation without a profile, one closing line saying the user can ask for more or less detail.

A clarification reply names the two or three likely sticking points and offers to expand one, or explains the part the user chose.

A profile suggestion is a short, plain-text block covering the user's level by subject, preferred style, and things to avoid, followed by one line telling the user to save it. For example:

```text
ez-understand profile
Level: beginner in finance; expert in software
Style: analogies, short bullet points
Avoid: math notation
```

## Completion

Each explanation finishes when the explanation (and any profile suggestion) is delivered. Do not continue to other tasks, do research, or reopen the original task; wait for the user's next message.

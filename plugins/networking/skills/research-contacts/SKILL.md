---
name: research-contacts
description: >
  Research named people's public professional activity and produce engagement tips
  tailored to why the user is reaching out: job search, client prospecting, partner or
  business development, investor outreach, talent sourcing, or an intent the user
  describes themselves. Trigger on phrases like "engagement tips for [name]", "how should
  I approach [name]", "what does [name] post about", "research [name] at [company]",
  "when is [name] most active", "how do I warm up [name] before pitching", or when the
  user invokes /research-contacts. Produces a chat answer plus a .docx. Outputs
  observational tips only and never drafts the outreach message.
version: 0.1.0
argument-hint: "[contact name(s)] at [company] [optional: --intent ...]"
---

# Contact engagement research

Stage two of the networking workflow, and useful on its own. Given one or more named
people, research what they publicly post and engage with, then turn that into advice on
how and when to approach them.

The research is broadly the same whoever is asking. The advice is not. What counts as a
natural opening differs completely between someone hoping to join a person's team and
someone hoping to sell into it, so intent governs the entire tips section.

## Before starting

Read these three files in full:

- `${CLAUDE_PLUGIN_ROOT}/shared/intent-profiles.md`
- `${CLAUDE_PLUGIN_ROOT}/shared/hard-rules.md`
- `${CLAUDE_PLUGIN_ROOT}/shared/output-and-naming.md`

Then read `references/workflow.md` in this skill directory and follow it exactly.

## The rule that does not bend by default

Output is tips, not messages. Describe themes, engagement style, timing, and angles.
Do not write the message, the connection note, the comment, or a sample script, whatever
the intent and however the request is phrased.

This is flippable for a specific intent, but only through a saved rule in
`memory/learned-preferences.md` created by `/networking:feedback`. A request made in
conversation is not enough; point the user at the feedback command so the change is
deliberate and recorded. `shared/hard-rules.md` holds the full statement of this.

## Shape of the run

1. Load memory, resolve intent, confirm which people and which company.
2. Research each person's public activity across LinkedIn, X, and Threads.
3. Estimate the days and hours they are most active, converted to the user's zone.
4. Write intent-slanted tips per person.
5. Deliver a chat answer, then a `.docx` named for the contact or, for several, for the
   company.

## Tools

Search first for a foothold. Then use Claude in Chrome where connected to open public
profiles and scroll recent activity, since these platforms return little to a plain fetch.
Only public content, no login walls, and no interaction of any kind on the user's behalf.

## Arguments

`$ARGUMENTS` carries the contact names, the company, and optionally an intent:

```
/networking:research-contacts Jane Doe, John Smith at Acme Corp
/networking:research-contacts Jane Doe at Acme Corp --intent client-prospecting
/networking:research-contacts Jane Doe
```

Ask for the company when it is missing, since it disambiguates common names and grounds
the research. When the run follows company research in the same conversation, inherit the
intent and the company and say so rather than asking twice.

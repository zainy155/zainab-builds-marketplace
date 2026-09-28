---
name: networking
description: >
  Single entry point for all networking research. Routes a request to company research,
  contact engagement research, a combined end-to-end run, or the feedback loop, and asks
  only for what is missing. Trigger when the user invokes /networking, or says things
  like "networking research on [company]", "help me network into [company]", "who should
  I know at [company] and how do I approach them", "warm up [name] at [company]",
  "do the full networking workflow for [company]", or names a target without saying
  which stage they want.
version: 0.1.0
argument-hint: "[company and/or people] [optional: --intent job-search|client-prospecting|partner-bd|investor-outreach|talent-sourcing|your own words]"
---

# Networking: router

This is the front door to the whole plugin. Everything the plugin can do is reachable
from here, so the user never has to remember which subskill does what.

Do not re-implement any research here. This skill decides which subskill runs, hands it
the parsed arguments, and where a run spans both stages, carries the intent and the
company forward so nothing is asked twice.

## Step 1: Read the shared context

Read these three files in full before routing, so the intent is resolved once and
inherited by whatever runs next:

- `${CLAUDE_PLUGIN_ROOT}/shared/intent-profiles.md`
- `${CLAUDE_PLUGIN_ROOT}/shared/hard-rules.md`
- `${CLAUDE_PLUGIN_ROOT}/shared/output-and-naming.md`

Load memory as `output-and-naming.md` describes. Resolve the intent by the order in
`intent-profiles.md`, and state it in one line before any research starts.

## Step 2: Parse `$ARGUMENTS`

Pull out whatever is present, in any phrasing and any order:

- **Companies.** One or more names. Five is the cap for a single run.
- **People.** Named individuals, usually with "at [company]".
- **Intent.** After `--intent`, or stated plainly ("for prospecting", "I want to sell
  them observability tooling").
- **Departments and locations.** Optional narrowing for company research.
- **Feedback.** A critique of a previous output rather than a new request.

## Step 3: Route

Pick the first branch that matches.

| What the arguments contain | Run |
|---|---|
| A critique of an earlier output, or an instruction to remember or stop doing something | `feedback` |
| Named people, with or without a company | `research-contacts` |
| Companies only | `research-companies` |
| Companies plus an explicit request to go end to end ("full run", "and how do I approach them", "then get me tips") | `research-companies`, then `research-contacts` on the shortlist |
| Nothing usable | Step 4 |

Say which branch is running and why, in one line, before starting: "Running company
research on Acme Corp for client prospecting." A user who meant something else can stop
you before the research spends time.

Then follow that subskill's `SKILL.md` and its `references/workflow.md` exactly. The
router changes nothing about how a subskill behaves, including the tips-not-messages
rule in `shared/hard-rules.md`.

## Step 4: When nothing usable was given

A bare `/networking` with no arguments is common and is not an error. Ask, in one
question, what the user wants:

- Research a company, its momentum, and the people worth knowing
- Get engagement tips for people already named
- Both, end to end, starting from a company
- Teach the plugin something about how the last output was wrong

Take the intent in the same exchange when memory has no `last_intent`, offering the five
presets and making clear a freeform intent is equally welcome. One round of questions,
then run.

## Step 5: Chain cleanly

On an end-to-end run, after company research delivers its shortlist:

1. Name the people you propose to research, drawn from the top of the shortlist. Default
   to the top three to five and let the user cut or add before you continue.
2. Carry the intent, the company, and any standing context into contact research without
   asking again. Say that you are inheriting them.
3. Deliver each stage's chat answer and `.docx` as that subskill specifies. The router
   adds no document of its own.

## Step 6: Close the loop

End any research run by pointing at the feedback route in one line, phrased as an
invitation rather than boilerplate: "If the shortlist skewed wrong, tell me with
`/networking:feedback` and I will keep it that way."

## Arguments

```
/networking Acme Corp --intent client-prospecting
/networking Acme Corp, engineering and product, Dubai office
/networking Jane Doe, John Smith at Acme Corp
/networking Acme Corp, full run, I want to sell them observability tooling
/networking the last shortlist was full of VPs who can't buy anything
/networking
```

The subskills remain directly callable for anyone who prefers them:
`/networking:research-companies`, `/networking:research-contacts`,
`/networking:feedback`.

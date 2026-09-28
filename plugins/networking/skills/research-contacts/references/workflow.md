# Stage two workflow: contact engagement tips

Full procedure for `/research-contacts` and any equivalent request in plain language.

## Step 0: Memory, intent, and disambiguation

Load `intent-defaults.md` and `learned-preferences.md` from the resolved memory directory.
Apply every rule scoped `global`, `intent:<current>`, or `skill:research-contacts`.

Resolve the intent. When this run follows company research in the same conversation,
carry that intent over and say so rather than asking again.

Confirm who is being researched. Ask for the associated company if it was not given.
When a name is common or several people match, ask the user to identify the right person
by title or LinkedIn URL before spending research effort on the wrong one.

## Step 1: Public activity research

For each contact:

- **LinkedIn.** Recent posts, articles, comments, and reactions. What they choose to
  write about versus what they only react to. Tone and register. Any recent role change,
  work anniversary, or announcement, all of which are natural and time-limited openings.
- **X.** Recent posts and replies, topics, tone, how much they engage versus broadcast.
- **Threads.** The same, where the person is active there.
- **Elsewhere, where it is public and professional.** Conference talks, podcast
  appearances, bylines, newsletters, public repositories. These often reveal more than
  social posts and tend to be underused by everyone else approaching the person.

- **Key themes.** The two to four topics they return to. Be specific. "Talks about
  hiring" is weak; "argues repeatedly that take-home tests waste candidates' time" gives
  the user something real to work with.

- **Engagement style.** Do they post original material or mostly comment? Long-form or
  short? Do they reply to strangers? Do they engage with questions more than praise?

- **Most active times.** From visible post timestamps and cadence, estimate the days and
  hour ranges when they are most active. Convert to the user's zone and name it. When the
  data is too sparse to support an estimate, say that instead of inventing a window.

- **LinkedIn URL** and **work email** where publicly findable, for example from a company
  site, a byline, a speaker page, or a public repository. A pattern-guessed address is
  labelled `inferred, unconfirmed` and never presented as verified.

- **Boundaries.** Anything they have publicly said about how they want to be contacted,
  including refusals. This gets surfaced prominently, and it outranks the intent.

## Step 2: Tips, slanted to the intent

For each contact, write bullet-point tips covering four things. What differs by intent is
the content, not the structure.

**A natural reason to engage.** Grounded in something the person actually published, not
a generic compliment.

- `job-search`: a theme where the user has real experience or a genuine question, ideally
  connected to the team's current work.
- `client-prospecting`: a problem this person has described in public that the user's work
  speaks to. Their words, their framing.
- `partner-bd`: a place where the two organisations' surfaces already touch, or an
  ecosystem view the person has argued for.
- `investor-outreach`: a thesis or market view they have published that the user's traction
  bears on.
- `talent-sourcing`: the work they have chosen to make public, and what about the role
  connects to it.
- Freeform: whatever answered question four of the derivation recipe.

**How they prefer to be engaged.** Drawn from observed behaviour. Whether to comment
first or approach directly, whether they respond to questions, what register fits.

**Timing.** Best days and hour ranges in the user's zone, plus any time-limited opening
such as a recent role change, a launch, or a funding announcement. For
`client-prospecting` and `investor-outreach`, note the trigger-event window explicitly,
since responsiveness in those intents is strongly time-dependent.

**What to avoid.** A position they have publicly pushed back on. An angle so obvious that
everyone else is already using it. Anything the intent profile lists under steer away.
Anything in the boundaries found in Step 1.

Then stop. No message, no note, no comment, no sample script, unless an active rule in
`memory/learned-preferences.md` scoped to this exact intent permits drafting, in which
case say in the output that a saved preference enabled it.

## Output: chat answer

One line naming the intent, then a table with one row per contact:

Contact name | LinkedIn URL | Work email (or `not found` / `inferred, unconfirmed`) |
Key themes | Most active days and times, in the user's zone | Engagement tips

Follow the table with the confidence note: which profiles could not be read, whether
browser access was available, and which estimates are thin.

## Output: docx

Same content, built with the `docx` skill using the header block from
`shared/output-and-naming.md`. One contact:

```
{Contact Name}_{YYYY-MM-DD}_{HHMM}.docx
```

Two or more:

```
Contacts_{Company Name}_{YYYY-MM-DD}_{HHMM}.docx
```

Deliver with `SendUserFile` and commit to the user's connected folder where one exists.

## Closing

The natural next step is the user writing their own approach, which this plugin
deliberately leaves to them. Close with the one-line feedback invitation.

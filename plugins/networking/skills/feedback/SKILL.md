---
name: feedback
description: >
  Capture the user's critique of a networking research output and turn it into a durable
  rule the plugin applies on every future run. Trigger on phrases like "that shortlist
  was too junior", "the tips missed the point", "stop including [x] in the brief",
  "always include [y]", "remember that I sell to [audience]", "that was wrong because",
  or when the user invokes /networking:feedback. Writes scoped rules to the plugin's
  memory file after showing the user a diff.
version: 0.1.0
argument-hint: "[what was off, in your own words]"
---

# Feedback and self-improvement

This is how the `networking` plugin gets better at serving one particular person. The
user says what was off, this subskill turns it into a rule, and both research subskills
read those rules before every run.

Read `${CLAUDE_PLUGIN_ROOT}/shared/output-and-naming.md` first for the memory path
resolution, and `${CLAUDE_PLUGIN_ROOT}/shared/hard-rules.md` for the boundaries a
preference cannot cross.

## Step 1: Understand what went wrong

Take the user's feedback together with the run it refers to, usually the most recent
research in the conversation. When the reference is unclear, ask which output they mean
rather than guessing and saving a rule against the wrong context.

Separate three things, because they lead to different places:

- **A durable preference.** "Shortlists should skew senior for prospecting." This becomes
  a rule.
- **A one-off correction.** "You got Jane's title wrong." Fix it now; do not save a rule.
  A factual error is not a preference.
- **A structural change.** "Add a section on competitor tooling." This becomes a rule if
  it can be expressed as an instruction the research subskills can follow. If it needs
  new research machinery, say plainly that it needs a plugin edit rather than a
  preference, and offer to describe the change.

## Step 2: Draft the rule

Write each rule so it survives out of context. Six months from now, in a different
conversation, the rule is all that remains of this exchange.

- **Imperative and specific.** "Rank the shortlist by budget authority before seniority"
  beats "be smarter about ranking".
- **One rule per idea.** Feedback carrying three complaints becomes three rules.
- **Scoped.** Every rule carries exactly one scope:
  - `global`: applies to every run
  - `intent:<name>`: applies only under that intent, for example
    `intent:client-prospecting`
  - `skill:<name>`: applies only to `research-companies` or `research-contacts`
  
  Default to the narrowest scope that fits. A complaint about a prospecting shortlist is
  almost never a global rule, and over-broad rules are the main way a memory file turns
  into noise.
- **Testable.** A future run should be able to tell whether the rule was followed.

## Step 3: Check it against the hard rules

A preference cannot override `shared/hard-rules.md`, with one exception. The tips-only
rule is flippable per intent, and only per intent. A rule that permits drafting must be
scoped `intent:<name>`, never `global`, and the user must be told plainly what they are
turning on before it is written.

Refuse, with a short explanation, any rule that would mean fabricating detail, dropping
confidence labels, ignoring a person's stated boundary, or accessing anything behind a
login. Offer the closest acceptable version instead.

## Step 4: Show the diff, then write

Show the user exactly what will be added, in the format below, and wait for their
confirmation before writing anything. Never write silently.

Handle conflicts by superseding, not deleting. When a new rule contradicts an existing
one, mark the old rule `superseded` with the date and keep it in place. History explains
why the plugin behaves as it does, and a rule that was right once is often worth
restoring.

Append to `learned-preferences.md` in the resolved memory directory in this format:

```markdown
### R012: Rank prospecting shortlists by budget authority
- **Scope:** intent:client-prospecting
- **Added:** 2026-08-23
- **Rule:** In the shortlist, rank people by budget authority over the problem first,
  then by seniority. A director who owns the line item outranks a VP who does not.
- **From:** "the shortlist was full of VPs who can't buy anything"
- **Status:** active
```

Use sequential IDs. When the memory directory is not writable, say so, show the user the
exact block to paste into `~/Downloads/networking/memory/learned-preferences.md`, and do
not pretend it was saved.

## Step 5: Confirm concretely

Tell the user what changes next time, in one or two sentences, in terms of output rather
than file contents. "Next prospecting run, the shortlist leads with budget owners and
VPs without a line item drop below them."

## Standing context

Some feedback is not a rule about behaviour but a fact about the user: what they sell,
which roles they hire, their time zone, the markets they cover. That belongs in
`intent-defaults.md` under standing context, not in the rules file. Both research
subskills read it, and it saves the user repeating themselves every run.

## Arguments

`$ARGUMENTS` is the feedback itself, in the user's own words:

```
/networking:feedback the shortlist was full of VPs who can't actually buy anything
/networking:feedback stop guessing email addresses, leave the cell blank instead
/networking:feedback remember I sell compliance software to mid-market fintechs
```

When `$ARGUMENTS` is empty, ask what was off about the last output. When it is unclear
which run the feedback refers to, ask before saving anything: a rule saved against the
wrong context is worse than no rule, because it will quietly distort future runs.

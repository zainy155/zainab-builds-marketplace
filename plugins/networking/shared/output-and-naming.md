# Output, naming, and memory paths

Both research subskills follow this file for delivery. Read it before building anything.

## Resolving the memory directory

The plugin carries `memory/` as a seed. When the plugin is installed, that copy usually
sits in a read-only synced directory, so the skill cannot write to it. Resolve the
writable location in this order and use the first that works:

1. `MEMORY_DIR` if the user has set it in the environment.
2. `~/Downloads/networking/memory/` on the user's own machine, reachable through the
   desktop bridge (`device_bash`, `device_list_dir`, `device_stage_files`,
   `device_commit_files`). This is the normal case: it is the source folder the plugin
   was built from and it stays writable.
3. `${CLAUDE_PLUGIN_ROOT}/memory/` as a last resort, read-only in most installs.

Read `intent-defaults.md` and `learned-preferences.md` from whichever location resolves.
If none is readable, carry on with no memory, say so in one line, and do not fail the
run. Only writes need a writable path; a missing memory file is an empty memory, not an
error.

## Reading memory at the start of a run

Every run begins by loading both files:

- `intent-defaults.md` supplies `last_intent`, `timezone`, and any standing context the
  user has saved, such as what they sell or the roles they target.
- `learned-preferences.md` supplies rules the user has taught the plugin through
  feedback. Apply every rule whose scope matches the current run: `global` always,
  `intent:<name>` when the intent matches, `skill:<name>` when the subskill matches.

When a preference visibly changes the output, mention it once in the chat answer. Silent
behaviour changes make the plugin feel unpredictable and stop the user from correcting a
rule that has aged badly.

## Time zone

Resolve the user's zone in this order:

1. `timezone` in `intent-defaults.md`.
2. The device's own zone through the bridge, for example `date +%Z` and `date +%z` run
   via `device_bash`.
3. Ask, and offer to save the answer to `intent-defaults.md`.

Get the current time by running a command, never by estimating:

```bash
TZ="$USER_TZ" date "+%Y-%m-%d %H%M"
```

Every timestamp the user sees comes from that zone: filename stamps, the docx header,
and the "most active" windows in contact research. When converting a post timestamp from
another zone, show the converted time and name the zone once, for example
`Tue 09:00-11:00 (Asia/Dubai)`.

## Chat answer comes first

Always present the full answer in chat before building the file. The user reads in the
conversation and files the document. An answer that exists only inside an attachment
forces a download to learn whether the research was any good.

The chat answer opens with one line naming the intent in use and where it came from.

## The docx file

Build the document with the `docx` skill available in the environment. Headings, real
tables, consistent spacing. Not a text dump inside a Word wrapper.

Every document carries a header block with:

- Subject: the company name, or the contact name or names
- Intent: the resolved intent, preset name or the freeform phrasing
- Prepared: the date and time in the user's zone, with the zone named
- Sources: how research was gathered, meaning web search only or web search plus browser
- Confidence note: one line on the limits of this particular run

### Filenames

Company research:

```
{Company Name}_{YYYY-MM-DD}_{HHMM}.docx
```

Contact research, one contact:

```
{Contact Name}_{YYYY-MM-DD}_{HHMM}.docx
```

Contact research, two or more contacts:

```
Contacts_{Company Name}_{YYYY-MM-DD}_{HHMM}.docx
```

The date and time come from the user's zone via the command above. Replace characters
that are unsafe in a filename, meaning `/ \ : * ? " < > |`, with a hyphen, and otherwise
leave names readable, spaces included. Examples:

Comparison document, when more than one company was researched in a single run:

```
Comparison_{YYYY-MM-DD}_{HHMM}.docx
```

Examples:

```
Acme Corp_2026-08-23_1432.docx
Jane Doe_2026-08-23_1508.docx
Contacts_Acme Corp_2026-08-23_1512.docx
Comparison_2026-08-23_1520.docx
```

## When the docx skill is missing

If no docx-capable skill is available, do not silently drop the file. Say so in one line,
deliver the full content as Markdown using the same structure and filename stem, and tell
the user that installing a docx skill restores the Word output. A missing dependency is
not a reason to lose the deliverable.

## Delivery

Send the file into the conversation with `SendUserFile`. When the user has a folder
connected, also write it there with `device_commit_files` and say in plain language which
folder it landed in. If no folder is connected, the chat card is the delivery and the
user can save it wherever they like, including as a Google Doc.

## Writing back to memory

At the end of a successful run, update `last_intent` in `intent-defaults.md` to the intent
just used, when the memory directory is writable. This is the only automatic write; every
other change to memory goes through `/networking:feedback` with the user's confirmation.

If the directory is not writable, skip the update silently. It is a convenience, not
something worth interrupting the user over.

## Closing every run

End with two short lines:

1. The natural next step for this intent. After company research that is usually contact
   research on named people from the shortlist. After contact research it is usually the
   user writing their own approach, which this plugin deliberately does not do for them.
2. An invitation to correct the output: "If any of this missed, run
   `/networking:feedback` with what was off and I'll remember it."

Keep the invitation to one line. It is there so the feedback loop actually gets used, not
to solicit praise.

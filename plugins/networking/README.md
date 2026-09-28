# Networking

Research a company and the people inside it, then get engagement tips for the ones worth
approaching. Every output is shaped by one input the plugin asks for up front: your
**intent**.

The same company looks different depending on why you are reaching out. A job seeker
wants to know whether the team is growing and who the hiring manager is. Someone selling
into that company wants to know who controls the budget and what they have publicly
complained about. This plugin treats that difference as the main input rather than an
afterthought.

## Intents

Five presets ship with the plugin:

| Intent | Question it answers |
|---|---|
| `job-search` | Is this a good moment to pursue this team, and who decides? |
| `client-prospecting` | Is this a live opportunity, what triggered it, and who owns the budget? |
| `partner-bd` | What partnership shape fits how they already partner, and is the slot taken? |
| `investor-outreach` | Does the thesis fit, are they deploying, and is there a conflict? |
| `talent-sourcing` | Is this team stable or loosening, and where is the movable talent? |

You are not limited to these. Describe your intent in your own words and the plugin
builds a matching profile using the derivation recipe in `shared/intent-profiles.md`,
states it back to you, and proceeds once you confirm.

Your last intent is remembered and used as the default. The plugin always says when it is
falling back to a remembered value, so a stale default is easy to catch.

## Skills

One command runs the whole plugin:

```
/networking Acme Corp --intent client-prospecting
/networking Jane Doe, John Smith at Acme Corp
/networking Acme Corp, full run, I want to sell them observability tooling
/networking
```

`/networking` reads what you gave it, says which stage it is running, and asks only for
what is missing. A bare `/networking` with no arguments asks what you want and gets on
with it.

| Skill | What it does |
|---|---|
| `/networking` | The front door - routes to any of the below, and chains company research into contact research on request |
| `/networking:research-companies` | Company momentum, a people map, an intent-framed read, and a ranked shortlist |
| `/networking:research-contacts` | Public activity research on named people, turned into engagement tips |
| `/networking:feedback` | Teaches the plugin what to do differently next time |

The subskills stay directly callable if you prefer them. Plain language works too.
"Research Acme for prospecting" and "how should I approach Jane Doe at Acme" both trigger
the right skill.

## What you get back

Every research run produces a chat answer first, then a Word document with the same
content. The chat answer is where you judge the research; the document is what you keep.

Filenames carry the subject and the timestamp in your own time zone:

```
Acme Corp_2026-08-23_1432.docx
Jane Doe_2026-08-23_1508.docx
Contacts_Acme Corp_2026-08-23_1512.docx
```

Each document opens with a header naming the intent that produced it, so a brief found on
disk weeks later still explains itself.

## Tips, not messages

The plugin never drafts your outreach. It tells you what someone posts about, how they
engage, when they are active, and what angle is natural. You write the message.

This is deliberate. Research scales; a message that sounds like you does not. The rule
can be turned off for one specific intent through the feedback loop, never globally and
never by asking mid-conversation, so switching it on is always a decision you made on
purpose.

## Getting better over time

```
/networking:feedback the shortlist was full of VPs who can't actually buy anything
```

The plugin turns that into a scoped rule, shows you the exact text before saving, and
applies it on every matching run afterwards. Rules are scoped `global`, to one intent, or
to one skill, and default to the narrowest scope that fits. Conflicting rules are marked
superseded rather than deleted, so the file explains itself later.

Facts about you rather than instructions, such as what you sell or which roles you hire,
go in `memory/intent-defaults.md` under standing context. Filling that in sharpens the
research more than almost anything else.

### Where memory lives

Installed plugins usually sit in a read-only directory, so the plugin cannot rewrite
itself in place. Memory resolves in this order:

1. `MEMORY_DIR` if you set it
2. `~/Downloads/networking/memory/` on your own machine, reachable through the desktop
   bridge, which is the normal case
3. the copy inside the plugin, read-only in most installs

Keep the source folder at `~/Downloads/networking` and the loop works without any setup.
Rules take effect on the next run. Changes to the skill files themselves need the plugin
reinstalled.

## Requirements

- A **docx-capable skill** in your environment, such as Cowork's built-in `docx` skill,
  for the Word documents.
- **Claude in Chrome**, optional but strongly recommended. LinkedIn, X, and Threads render
  through JavaScript and give a plain fetch almost nothing. With the extension connected,
  the plugin reads public profiles and recent activity directly. Without it, the research
  falls back to web search and flags what it could not confirm.

No API keys or environment variables are required.

## Install

**Claude Desktop or Cowork.** Open **Customize** in the left sidebar, go to the
**Plugins** tab, and upload `networking.zip`. In Cowork, open the **Cowork** tab first,
then **Customize**. Plugins you add this way are saved locally on your computer.

**Claude Code, for testing.** Point at the folder directly:

```bash
claude --plugin-dir ./networking
```

`--plugin-dir` also accepts the zip. Run `/reload-plugins` after editing a file to pick up
changes without restarting.

**Claude Code, as an install.** Register the folder as a one-plugin marketplace, then
install from it. See Sharing below for the `marketplace.json`.

## Sharing it with other people

**One person.** Send them `networking.zip`. They upload it under Customize, Plugins, the
same way you installed it. Nothing else is needed.

**A team, with updates.** Publish a marketplace. Put the plugin folder in a git repository
and add a catalog file at `.claude-plugin/marketplace.json` in the repository root:

```json
{
  "name": "your-marketplace-name",
  "owner": { "name": "Your Name" },
  "plugins": [
    {
      "name": "networking",
      "source": "./networking",
      "description": "Intent-driven company and contact research for networking outreach"
    }
  ]
}
```

Layout:

```
your-repo/
├── .claude-plugin/
│   └── marketplace.json
└── networking/
    ├── .claude-plugin/plugin.json
    ├── skills/
    ├── shared/
    └── memory/
```

Others then add the marketplace once and install from it. In Desktop or Cowork: Customize,
Plugins, the **+** button in Personal plugins, Add marketplace, Add from a repository. In
Claude Code:

```
/plugin marketplace add your-org/your-repo
/plugin install networking@your-marketplace-name
```

To ship an update, bump `version` in `plugin.json` and push. Users pull it with
`/plugin marketplace update`. Keep the marketplace repository private to keep the plugin
internal to your team.

Validate before you share:

```bash
claude plugin validate ./networking
```

## A note on accuracy and ethics

Research draws only on publicly available information. Anything unverified is labelled
rather than presented as fact, and the plugin does not invent people, titles, or dates.
It never scrapes behind a login, never sends a connection request or message, and never
takes any action on a platform on your behalf. If someone has publicly said they do not
want to be approached, the brief says so and recommends against it, whatever your intent.

`shared/hard-rules.md` holds the full set, and a learned preference can never override it.

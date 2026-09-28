---
name: research-companies
description: >
  Research a company's momentum and map the people worth knowing inside it, with the
  research slanted to the user's stated intent: job search, client prospecting, partner
  or business development, investor outreach, talent sourcing, or any intent they
  describe in their own words. Trigger on phrases like "research [company]", "map out
  [company]'s [department] team", "who should I talk to at [company]", "is [company] a
  good prospect", "help me prospect [company]", "research [company] before I reach out",
  "who owns [function] at [company]", or when the user invokes /research-companies.
  Produces a chat answer plus a .docx brief.
version: 0.1.0
argument-hint: "[company name] [optional: --intent job-search|client-prospecting|partner-bd|investor-outreach|talent-sourcing|your own words] [optional: departments/locations]"
---

# Company and people research

Stage one of a two-stage networking workflow. This subskill answers two questions about
a target company: what is going on there right now that changes how or whether to reach
out, and which specific people are worth approaching.

Intent decides both answers. The same company researched for a job search and for client
prospecting should produce visibly different briefs, because the signals that matter and
the people who matter are different.

## Before starting

Read these three files in full. They are not optional context.

- `${CLAUDE_PLUGIN_ROOT}/shared/intent-profiles.md`
- `${CLAUDE_PLUGIN_ROOT}/shared/hard-rules.md`
- `${CLAUDE_PLUGIN_ROOT}/shared/output-and-naming.md`

Then read `references/workflow.md` in this skill directory and follow it exactly. It
holds the step-by-step research procedure and the output structure.

## Shape of the run

1. Load memory and resolve the intent. Restate it in one line.
2. Research company momentum through the lens that intent sets.
3. Map people in the target departments and locations, defaulting to the roles the intent
   profile names.
4. Turn the two into an intent-framed read and a ranked shortlist, which is the part the
   user actually acts on.
5. Deliver a chat answer, then a `.docx` named `{Company Name}_{YYYY-MM-DD}_{HHMM}.docx`.
6. Offer contact research on named people from the shortlist.

## Tools

Use `WebSearch` and web fetching for news, filings, company pages, and funding coverage.

When Claude in Chrome is connected, use it for LinkedIn company and people pages. These
render through JavaScript and return almost nothing useful to a plain fetch, so browser
access is the difference between a real people map and a thin one. When it is not
connected, work from search results, say so in the confidence note, and mark the gaps.

## Arguments

Anything the user types after the skill name arrives as `$ARGUMENTS`. Parse it for three
things, in any order and in whatever phrasing the user used:

- **The company name.** Required. Ask if it is missing.
- **An intent.** Given after `--intent`, or simply stated in plain language, such as
  "for prospecting" or "I want to sell them analytics". Both forms are equally valid.
- **Departments and locations.** Optional. When absent, take the default target roles
  from the intent profile and say which defaults are in use so the user can redirect
  cheaply.

Examples that must all work:

```
/networking:research-companies Acme Corp --intent client-prospecting
/networking:research-companies Acme Corp, engineering and product, Dubai office
/networking:research-companies Acme Corp I want to sell them observability tooling
/networking:research-companies Acme Corp
```

The last one starts momentum research immediately, resolves the intent from memory or by
asking, and requests departments while that research runs.

## More than one company

The skill accepts a list, which is the common case for prospecting and investor research:

```
/networking:research-companies Acme Corp, Globex, Initech --intent client-prospecting
```

With two or more companies, run the workflow for each, then add a comparison the single
company case does not need:

- A ranking of the companies against the intent, with one line of reasoning each.
- What separates the top from the bottom, stated as the deciding factor rather than a
  score.
- A recommendation on where to spend effort first, and which to drop for now.

Cap a single run at five companies. Beyond that the research thins out and the comparison
stops being useful; ask the user to prioritise instead of quietly doing a worse job on ten.

Deliverables for a multi-company run: one chat answer covering all of them, and one docx
per company using the standard naming, plus a comparison document named
`Comparison_{YYYY-MM-DD}_{HHMM}.docx`.

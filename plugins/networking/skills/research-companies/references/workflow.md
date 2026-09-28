# Stage one workflow: company and people

Full procedure for `/research-companies` and any equivalent request in plain language.

## Step 0: Memory and intent

Load `intent-defaults.md` and `learned-preferences.md` from the resolved memory
directory (`shared/output-and-naming.md` holds the resolution order). Apply every rule
scoped `global`, `intent:<current>`, or `skill:research-companies`.

Resolve the intent using the order in `shared/intent-profiles.md`. Load that profile, or
derive one for a freeform intent using the five-question recipe there and state it back
before proceeding.

Confirm the target departments and locations. If the user gave none, take the default
target roles from the intent profile and say which defaults are being used, so they can
redirect cheaply. Momentum research in Step 1 does not depend on this and can start
immediately either way.

## Step 1: Company momentum, through the intent lens

Every intent gets this base layer:

- Hiring and headcount direction, including open-role volume and any layoffs, freezes, or
  restructuring.
- Mergers and acquisitions in either direction, with the counterparty, deal size where
  public, and the stated rationale.
- Funding raised or investments made, with round, amount, and lead investors. For each
  one, identify which department, product line, or project it was tied to, because that
  is where money and headcount actually land.
- Leadership changes.

On top of that, research the signals the intent profile names as priorities. For
`client-prospecting` that means budget and spend triggers, vendor churn, and publicly
stated pain. For `partner-bd` it means existing partnerships, integrations, and channel
overlap. For `investor-outreach` it means thesis, stage, recent deals, and portfolio
adjacency. For `talent-sourcing` it means attrition signals and public technical output.
For a freeform intent, it means whatever answered question three of the derivation recipe.

Prioritise sources from the last twelve months and record the publication date of every
claim. Use news sites, press releases, the company newsroom and blog, filings, LinkedIn
company updates, and funding coverage surfaced through search.

Then write the synthesis: what these signals together suggest about the company's current
priorities and which teams are growing or shrinking. Keep it to a few sentences of prose.
Do not restate the bullet list in paragraph form.

## Step 2: Map departments and people

For the target departments and locations:

1. Establish which teams the company actually has there, using about and careers pages,
   org-chart-style coverage, LinkedIn company pages, and press mentions.
2. For each person found, collect: full name, business title, location as far as it can be
   confirmed, education and qualifications, notable career highlights such as promotions,
   projects, publications, talks, or awards, and past employers showing the trajectory.
3. Add one field the job-search-only ancestor of this plugin did not have: **why this
   person matters for this intent**. One clause. For `client-prospecting` that might be
   "owns the budget line for the function". For `job-search`, "hiring manager for the
   target role". This field is what makes Step 3 possible.
4. Use Claude in Chrome where connected to open public LinkedIn profiles and the company
   People tab. Search snippets alone produce a thin, stale map.
5. Do not invent people or details. A department with little public information gets a
   sentence saying so, not a padded list.

## Step 3: Intent-framed read and shortlist

This is the section the user acts on, and it is the main structural addition over a plain
people table.

**The read.** Three to five sentences answering the question the intent actually asks:

- `job-search`: is this a good moment to be pursuing this team, and what would a strong
  candidate emphasise right now?
- `client-prospecting`: is this a live opportunity, what is the likely trigger, and what
  is the entry point?
- `partner-bd`: what partnership shape fits how this company already partners, and is the
  slot occupied?
- `investor-outreach`: does the thesis fit, is the fund deploying, and is there a conflict?
- `talent-sourcing`: is this team stable or loosening, and where is the movable talent?
- Freeform: the outcome named in question one of the derivation recipe.

Be willing to conclude that the answer is no. A brief that says "this is not the moment,
here is what would change that" is worth more than one that dutifully ranks people at a
company the user should skip this quarter.

**The shortlist.** The top three to five people, ranked by relevance to the intent, each
with one line on why they rank there and one line on the opening angle. The full table
still follows, but the shortlist is what gets used.

## Output: chat answer

In this order:

1. One line: the intent in use and where it came from.
2. The momentum synthesis as prose, with source dates.
3. The intent-framed read.
4. The ranked shortlist.
5. The full people table: Name, Title, Location, Department, Background, Past employers,
   Career highlights, Why they matter for this intent.
6. The confidence note, covering what could not be confirmed and whether browser access
   was available.

## Output: docx

Build the same content with the `docx` skill, using the header block and filename rules
in `shared/output-and-naming.md`:

```
{Company Name}_{YYYY-MM-DD}_{HHMM}.docx
```

Deliver with `SendUserFile`, and commit it to the user's connected folder when one is
available.

## Multiple companies in one run

When the user named more than one company, repeat Steps 1 to 3 for each, then add a
comparison section: the companies ranked against the intent with one line of reasoning
each, the deciding factor that separates top from bottom, and a recommendation on where
to spend effort first. Produce one docx per company plus
`Comparison_{YYYY-MM-DD}_{HHMM}.docx`. Cap a run at five companies and ask the user to
prioritise beyond that.

## Handoff

Ask which people from the shortlist, or any other named contact, the user wants
engagement tips for, then move to the `research-contacts` subskill with the intent
carried over. Close with the one-line feedback invitation.

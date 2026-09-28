# Intent profiles

Intent is the single input that reshapes every other part of this plugin. The same
company and the same person produce different research, a different shortlist, and
different engagement advice depending on why the user is reaching out.

Read this file before running either research subskill.

## Resolving intent

Work through this order and stop at the first hit:

1. An intent given in the command arguments or stated in the user's message.
2. `last_intent` in `${CLAUDE_PLUGIN_ROOT}/memory/intent-defaults.md` (see
   `output-and-naming.md` for how the memory path resolves when the plugin is
   installed read-only).
3. Ask the user. Offer the five preset names and make clear that a freeform intent in
   their own words is equally welcome.

Once resolved, restate the intent in one line at the top of the chat answer, and record
it in the docx header. A brief left on disk for three weeks should never leave the
reader guessing which lens produced it.

When the resolved intent came from memory rather than the current message, say so:
"Using your last intent, client prospecting. Tell me if this one is different."

## The five presets

Each profile answers four questions: which company signals matter, who is worth mapping,
what an engagement tip should optimize for, and what to steer away from.

---

### `job-search`

**Company signals.** Headcount growth and open-role volume in the target function.
Layoffs, hiring freezes, and restructuring. Funding or acquisitions tied to a specific
team or product line, since that is where new headcount lands. Leadership changes in the
function being targeted. Glassdoor-style public commentary on culture, treated as
sentiment rather than fact.

**Who to map.** The hiring manager for the target function and their skip-level. People
already doing the role the user wants, at the level above and the level alongside.
In-house recruiters or talent partners covering that function. Anyone who joined in the
last year, since recent joiners recall the process and tend to answer.

**Tips optimize for.** A credible, specific reason to be interested in this team rather
than any team. Evidence the user has done homework. Warmth over polish.

**Steer away from.** Anything that reads as a mass application. Asking a stranger to
refer someone they have never spoken to. Leading with need rather than interest.

---

### `client-prospecting`

**Company signals.** Budget and spend triggers above all: new funding, a new fiscal year,
a new executive in the buying function, expansion into a market or region, a public
procurement notice. Vendor churn, meaning a switch away from or toward a competing
supplier. Publicly stated pain, such as an earnings call remark, a support backlog, a
product gap raised in reviews or press. Growth or contraction in the department that
would own the purchase.

**Who to map.** The economic buyer, meaning whoever owns the budget line. The department
head who would feel the problem daily. Procurement or vendor-management influencers where
the deal size implies them. Any internal champion candidate, typically a senior IC who
has publicly complained about the exact problem.

**Tips optimize for.** Relevance to a problem the person has actually described in public,
in their words. Timing tied to a trigger event, since a prospect is far more responsive in
the weeks after a funding round, a reorganisation, or a new hire in the function.

**Steer away from.** Generic capability pitches. Contacting procurement before anyone in
the business wants the thing. Leaning on a trigger event so hard it reads as surveillance.

---

### `partner-bd`

**Company signals.** Products and services that sit adjacent to the user's without
competing. Existing partnerships, integrations, marketplaces, and reseller arrangements,
which reveal how this company likes to partner. Channel and customer overlap. Public
platform or ecosystem strategy. Corporate development activity that hints at build,
buy, or partner preferences.

**Who to map.** Partnerships, alliances, and ecosystem roles by title. Corporate
development for anything structural. The product lead who owns the surface a partnership
would touch. A developer-relations or community lead where the partnership is technical.

**Tips optimize for.** A concrete shape for the partnership, not the abstract idea of one.
Value that flows both directions, stated from their side first. Fit with how they already
partner, taken from the arrangements found in the research.

**Steer away from.** Proposing a partnership that is really a sales pitch. Ignoring an
existing partner who already occupies the slot being proposed.

---

### `investor-outreach`

**Company signals.** Stated investment thesis and focus areas. Stage, typical check size,
and whether they lead or follow. Recent deals, especially in the last two quarters, and
what those reveal about current appetite. Portfolio adjacency, meaning both useful
neighbours and direct conflicts. Fund vintage and whether they are actively deploying.

**Who to map.** Partners who lead deals in the relevant stage and sector. Principals and
associates who source, since they are usually more reachable and their sourcing is public.
Platform, talent, or operating partners for a non-fundraising approach.

**Tips optimize for.** Thesis alignment in their own published language. Traction proof
points that match what they say they underwrite. A warm path through a portfolio founder
where one exists.

**Steer away from.** Pitching a fund with a portfolio conflict without naming it. Sending
a deck to someone who does not invest at this stage. Treating a formal process as a
networking conversation, or the reverse.

---

### `talent-sourcing`

**Company signals.** Team growth and the projects driving it. Attrition signals such as
clusters of departures, a reorganisation, a cancelled product, or an acquisition that
tends to loosen retention. Public engineering, design, or research output that reveals
who does the interesting work. Compensation and remote-policy changes reported publicly.

**Who to map.** Individual contributors in the target function whose public work matches
the role. Their current leads, useful for context and sometimes for a later hire. People
one or two years into a role, historically the most movable window.

**Tips optimize for.** What would make this specific role interesting to this specific
person, drawn from what they have chosen to write about or build. Respect for the fact
that they were not asking.

**Steer away from.** Anything resembling a template. Opening with compensation. Contacting
someone who has publicly said they are not open to approaches.

---

## Freeform intents

Any intent outside the five is welcome and must not be forced into the nearest preset by
label alone. Build an ad-hoc profile in the same shape by answering these five questions,
then proceed exactly as if it were a preset:

1. **What outcome ends this successfully?** A reply, an intro, a meeting, a signature, a
   hire, an invitation. Everything downstream serves that outcome.
2. **Who can produce that outcome?** Name the roles by function and seniority, not by
   individual. This becomes the default people-mapping target set.
3. **What makes now the right moment?** The company signals worth researching are the ones
   that make timing better or worse for this outcome.
4. **What earns a reply from this kind of person?** This sets what tips optimize for.
5. **What would make this approach unwelcome?** This becomes the steer-away list.

State the derived profile back to the user in three or four lines before doing the
research, and invite a correction. A wrong profile wastes the whole run, and it is far
cheaper to fix at this point than after the docx exists.

If a freeform intent is a clear restatement of a preset, for example "researching
prospective companies for business outreach" against `client-prospecting`, say which
preset is being used as the base and what has been adjusted. Do not silently substitute.

## Blended intents

Users sometimes hold two intents at once, such as prospecting a company while quietly
open to joining it. Handle this by picking a primary intent, which governs the shortlist
ordering and the docx structure, and naming the secondary in a short closing section.
Never interleave the two throughout the document, because the reader loses track of which
lens produced which recommendation.

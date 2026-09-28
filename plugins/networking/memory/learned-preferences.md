# Learned preferences

Rules this plugin has been taught. Both research subskills read this file before every
run and apply each rule whose scope matches.

Add rules with `/networking:feedback "what was off"`. Editing by hand works too; keep the
format below so the subskills can parse it.

## Scopes

- `global`: every run
- `intent:<name>`: only under that intent, for example `intent:client-prospecting`
- `skill:<name>`: only for `research-companies` or `research-contacts`

Narrow scopes age better than broad ones. A complaint about one kind of run is rarely a
rule about all of them.

## Status values

- `active`: applied on every matching run
- `superseded`: kept for history, no longer applied, with the replacing rule noted

Rules are superseded rather than deleted, so the file explains why the plugin behaves the
way it does and an old rule can be restored.

## Format

```markdown
### R001: Short imperative title
- **Scope:** intent:client-prospecting
- **Added:** YYYY-MM-DD
- **Rule:** The instruction, specific enough that a later run can tell whether it was
  followed.
- **From:** the feedback that produced it, in your words
- **Status:** active
```

## Rules

_No rules yet. The first run of `/networking:feedback` will add one here._

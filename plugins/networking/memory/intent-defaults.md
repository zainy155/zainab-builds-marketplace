# Intent defaults and standing context

Read by every subskill at the start of a run. Edited by `/networking:feedback`, or by
hand whenever you like.

## Settings

```yaml
last_intent: job-search
timezone: Asia/Dubai
```

`last_intent` is updated after each run and used as the default when no intent is given
in the request. The plugin always says when it is falling back to this value, so a stale
default is easy to catch.

`timezone` governs every timestamp the plugin produces: filename stamps, the docx header,
and the estimated active windows for contacts.

## Standing context

Facts about you that save repeating yourself. Delete the placeholder lines and fill in
whichever apply. Anything left blank is simply ignored.

- **What I do:**
- **What I sell or offer:**
- **Roles or functions I target:**
- **Markets and regions I cover:**
- **Companies to always exclude:**
- **Anything else worth carrying between runs:**

## Notes

Adding standing context sharpens the research more than almost anything else. A
prospecting run that knows what you sell can judge whether a company is actually a fit,
rather than listing signals and leaving the judgement to you.

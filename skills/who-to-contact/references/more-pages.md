# Extra pages

Read this when the user answers the step 8 question in SKILL.md.

- "more" or "yes" means 10 more people, one page. A number of people means that many divided by 10, rounded up, in pages. "no" or 0: say "No more people fetched."
- Above 50 more people, call `get-usage` (free) and ask once: "That is about <people> credits of your <credits_remaining>, plus their work emails. Go ahead?"
- Fetch one page at a time with `cursor`, and the same `query`, `mode` and `tags`. Changing any of them returns an error.
- Drop bad records on each page as in step 5, and print that page's drop line. "Add them back" then covers the latest drop line only. If the same LinkedIn URL appears again, keep the first row only.
- Run step 6b on the new rows (and step 6, if the user has asked for X accounts in this conversation), show the new rows numbered after the last row shown, and rewrite the same CSV with them added.
- Stop when `next_cursor` is null, on 402 or on 429. Print "Added <n> more people.", the Saved line, and the totals for this turn as in step 7.

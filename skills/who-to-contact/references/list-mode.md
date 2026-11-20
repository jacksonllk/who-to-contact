# List mode

Read this when the user gives several companies at once: a CSV file, an attached file, or a pasted list. The rules for every run in SKILL.md still apply. Each row is a company. A list of people to enrich is a different job: say "This skill finds the people at companies. It does not enrich a list of people." and stop.

## 1. Read the list

- **File mode:** read the CSV file the user names or attaches. **Chat mode:** use the attached or pasted text.
- Find the columns by their headers, ignoring case:
  - Website: `domain`, `website`, `url`, `company domain`, `company website` or `website url`.
  - Company name: `company`, `company name`, `name`, `account`, `account name` or `organization`.
  - If neither is found, ask once which column holds the company. If none does, stop and say: "The list needs a column with each company's name or website."
- Clean each website as in SKILL.md step 1 (`https://www.Stripe.com/` becomes `stripe.com`).
- A repeated website or name is searched once. Its later rows are kept with `status` `repeated company` and empty person columns.
- Keep every input column. They are copied to the output unchanged. Never change the user's file.
- **Chat mode** handles at most 25 companies per run, so the output fits in one reply. **File mode** handles at most 100, because the file is read and written whole by hand. Above the limit, say so and offer to run the first 25 or 100.

## 2. Ask who to reach, then start

- Ask who to reach exactly as in SKILL.md step 1, once for the whole list. Skip it if the message already says. The answer is the go-ahead, as in SKILL.md.
- Call `get-usage` (free) before the first paid call.
- Up to 10 companies: start at once. Print "Searching <N> companies for <label>: <the titles, with "and" before the last>."
- More than 10 companies: ask once before starting, because a long list spends far more than one search:

> <N> companies, about 12 Context.dev credits each (up to 22 if a company needs a second search). You have <credits_remaining> credits. Reply yes to start.

- Write numbers of 1,000 or more with commas. If 22 credits per company is more than the balance, say how many companies fit and offer to run that many.
- Work emails follow SKILL.md step 6b: they are found automatically when the Enrichable tools are available, unless the user asked for no emails.

## 3. Search each company

One company at a time, in the list order:

1. **Identify it.** With a website, follow SKILL.md step 2, including the homepage check. With a name only, call `company-search` (2 credits) with the name. Use the result only when exactly one result comes back, or the top result's name equals the row's name (compared without case, spaces or punctuation). Otherwise mark the row `check this company` with empty person columns, and go to the next company. Never guess between companies.
2. **Search.** Call `people-search` with `<titles> at <domain>`, `mode: "fast"` and `tags`. One page.
3. **Retry** as in SKILL.md step 4: at the other domain, then by name. When step 1 found no name, use the row's company name. For `engineering`, when the retry also finds no one, search `founders` at the last domain or name searched.
4. Apply the SKILL.md step 5 drops for this company. Remove anyone whose LinkedIn URL already appeared for an earlier company.
5. **File mode:** write the whole output file again with this company's rows added, right away, so a stop keeps the work done so far.
6. After every 10 companies, print one line: "Done 10 of <N>."

After the last company, unless emails are skipped as in SKILL.md step 6b, run `references/emails.md` for all the people found, then write the file again with their emails.

On 402 or 429, stop. Mark the companies not yet searched as `not run`, report how many were done, and say: "Ask me to continue this list later. I will skip the companies already done."

## 4. Output

A new file, one row per person. Columns, in this order: every input column, then `status`, `name`, `title`, `seniority`, `function`, `role_started_on`, `linkedin_url`, `work_email`, `email_status`, `company_domain`. The person columns follow SKILL.md step 7. Leave out `work_email` and `email_status` when emails were not requested. Lists have no `x_url` column.

- `status` is `found` for a person row.
- A company with nobody left after the drops gets one row with its input columns, `status` `no matches` and empty person columns.
- `check this company`: the name matched several companies or none. The user can add the website and run the list again.
- `not run`: not searched because of a stop.

**File mode:** save to `who-to-contact/list-<persona>-<YYYY-MM-DD>.csv`, adding `-2`, `-3` and so on rather than overwriting. Create the folder and its `.gitignore` as in SKILL.md step 7. Print the Saved line as in step 7.

**Chat mode:** print the summary (section 5), then "Copy this into a .csv file." and the whole CSV in one code block. If your app can attach files, attach the CSV instead. Do the same at a stop.

## 5. Summary

Do not list every person in the chat. The file or the CSV holds them. Show one table: Company | People found | More matches. "More matches" is "yes" when `next_cursor` was not null. Then name the companies marked `check this company`, if any. Then the totals from `get-usage`, and the email line as in SKILL.md step 7 when emails were requested.

Say once: "X accounts and extra pages are only found for one company at a time. Ask about one company to get them." End with the closing line from SKILL.md. Add nothing after it: no review of each company or person. The user checks the file.

## 6. Continue a stopped list

When the user asks to continue, read the latest `who-to-contact/list-<persona>-…csv` (file mode) or the last CSV in this chat (chat mode). Search only the companies marked `not run`. Ask again first only when more than 10 are left.

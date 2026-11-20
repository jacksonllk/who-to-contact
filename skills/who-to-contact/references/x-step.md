# X accounts

Read this in step 6 of SKILL.md. The rules for every run in SKILL.md still apply. Print nothing during this step. Its results show in the step 7 table. To save time, work on up to 3 people at once (up to 3 `web-search` or `web-scrape` calls in flight), each person still going down the waterfall in order.

Send `tags: ["who-to-contact"]` on every call, as in SKILL.md. The company handle is the path of `socials.x` from step 2, for example `acme_hq` from `https://x.com/acme_hq`. When `socials.x` is null, look on the company's own website once per conversation: call `web-scrape` on `https://<domain>` with `formats: {parse: true}`, `parseParams: {type: "prebuilt", parser: "links"}`, `responseMode: "compact"` and `tags` (1 credit), and take the first link to an X profile (`x.com/<handle>` or `twitter.com/<handle>`, not a post, `/intent` or `/share` link). Most sites link their official account in the header or footer. If the site links none, the handle stays null. The company name is the step 2 name, or else the domain's first part. Never give a person the company's own X account. Compare handles without case, on x.com and twitter.com alike.

The search name is the person's name, except when people search shortened the last name to one letter ("Jane D."). Then read the full name from the LinkedIn URL when its slug starts with the first name and the next word (after one optional hyphen, up to the next hyphen or digit) has more than one letter and starts with the given initial: `linkedin.com/in/janedoe` gives "Jane Doe", but `linkedin.com/in/jane-d-a2a5` gives nothing. Capitalize each part. Otherwise keep the name as returned. A shortened name finds the wrong people. Use the full name only for searching. The table keeps the name as returned.

Two LinkedIn URLs match when their `/in/<slug>` parts are equal, ignoring case, `www.` and a trailing slash.

To pair links on a scraped page, first set aside the company's own X account. Look only at LinkedIn and X links, and ignore all other links. If the first LinkedIn link comes before the first X link, each X link belongs with the nearest LinkedIn link before it. Otherwise each X link belongs with the nearest LinkedIn link after it. An X link with no LinkedIn link on that side has no pair.

This step is a waterfall, like the email lookup. Each person goes down the sources below in order and stops at the first one that proves an account. The order puts first the source that has found the most: in our runs the X search found 6 of 9 accounts and the person's own or company page found 3. Every source still runs for anyone not yet found, so the order changes the cost, not what is found.

## 1. The company's team page (once per company, executive searches only)

Run this only when the persona is `founders` or any title is C-level (starting "Chief", or CEO, CTO, CFO, COO, President, Founder), and only once per company in a conversation: skip it for "add them back" and extra pages. Team pages mostly list founders and executives: in our tests it found no one else.

Company pages that list staff often put each person's LinkedIn link next to their X link. The LinkedIn link proves whose X account it is.

1. Call `web-map` with `domain`, `urlRegex: "(?i)(team|about|people|leadership|founders|company|<last names joined by |>)"` and `tags`. Use the last word of each search name, lower-cased, with regex special characters escaped. Leave out a last name of one letter.
2. Pick at most 2 URLs: a path of one part named team, about, people, leadership, founders or company (such as `/about` or `/team`), or a path that contains a person's last name. Never a docs, changelog, customer story or blog post page. A blog author page is fine. If more than 2 fit, take the shortest paths, with `/about` and `/team` first. If none fits, go to source 2.
3. Call `web-scrape` on each with `formats: {parse: true}`, `parseParams: {type: "prebuilt", parser: "links"}`, `responseMode: "compact"` and `tags`.
4. An X link (`x.com/<handle>` or `twitter.com/<handle>`) belongs to a person when its pair is a LinkedIn link that matches the person's LinkedIn URL.

## 2. Search X

1. Call `web-search` with `query: "\"<search name>\""` (the name in double quotes, with no company word), `includeDomains: ["x.com", "twitter.com"]`, `numResults: 10`, `responseMode: "compact"` and `tags`. The company word makes the search return posts about the company instead of the person.
2. Collect candidate handles from results that show the person's name as the account name, for example a title "Jane Doe (@janedoe) / X" or "Jane Doe on X". Take the handle from the URL `x.com/<handle>`, including the account's own pages such as `/with_replies`, `/reposts`, `/media` and `/status/<id>`. Skip `/i/`, `/search`, `/hashtag`, `/explore` and `/home`. List each handle once. Never take a handle from a post by someone else that only mentions the person.
3. Put first the candidates whose snippet names the employer or its handle. Scrape at most 2: `web-scrape` with `url: "https://x.com/<handle>"`, `formats: {markdown: true}` and `tags`. Never trust a snippet alone. It can be an old copy. Keep the default cache: a copy up to 3 days old is fine, because people rarely change jobs, and a fresh fetch costs the same.
4. Keep the first account where at least one signal passes in the scraped page. If none passes, go to source 3. The employer is the target company, or for a row added back from another company, `role.company.name` (such a company has no known handle, so only signal B can pass). The signal must show the current job: a mention after "ex-", "former", "formerly", "previously" or "past" does not count:
   - **A**: the bio contains `@<employer handle>`, or "@" followed by the employer's name, with or without a space after the "@". Compare without case. The match must end where the handle ends: `@linear` does not match `@linearb`. A company word without "@" does not count.
   - **B**: the bio shares an exact phrase of 3 or more words, or a link, with the person's headline from the search. A job title or the company name does not count as the phrase.
   - **C**: a link to `https://x.com/<employer handle>` sits right under the person's name. This is the affiliation badge.

## 3. The person's own page or a company author page

1. Call `web-search` with `query: "<search name> <company name>"`, `numResults: 5`, `excludeDomains: ["linkedin.com", "x.com", "twitter.com", "rocketreach.co", "zoominfo.com", "apollo.io", "wiza.co", "contactout.com", "signalhire.com", "lusha.com", "theorg.com", "theofficialboard.com", "seedtable.com", "truepeoplesearch.com", "fastpeoplesearch.com"]`, `responseMode: "compact"` and `tags`. Never pick or scrape a page from a site that sells contact data or lists people's profiles, even if it is not in this list.
2. Pick pages in this order, using the search name, and stop at the first that gives an account. Scrape at most 2, counting a scrape that fails. If none fits or none gives an account, the person gets no X account.
   1. A page on the company's domain with the person's name in its title or URL, such as a blog author page.
   2. A site whose domain contains the person's last name, such as `janedoe.dev`.
   3. Any other result whose title starts with the person's name and whose title or snippet names the company.
   4. The person's profile on another platform (such as skool.com, about.me, linktr.ee or github.com) whose title is their name and whose text names the company.
   Never pick an encyclopedia or wiki page (such as wikipedia.org or wikidata.org): they are about a famous namesake, not the person.
3. Scrape each with the `links` parser as in source 1.
4. First set aside the company's own X account. Then accept an X link whose pair is a LinkedIn link that matches the person's, as in source 1. On a page on the company's domain with no such pair, accept the X link only when it is the only one left and its link text or handle names the person (their first or last name). A company blog post about a person often links only its author's account. On a page outside the company's domain, accept an X link only when it is the only X link left and it pairs with the person's LinkedIn, or the page is the person's own profile from rule 4 above and has exactly one X link. Pages about several people, such as podcast and event pages, often put a host's X link next to a guest's LinkedIn link.

## Result

Write each accepted account as `https://x.com/<handle>`. Everyone else gets an empty X value. Show nothing about rejected hits.

On 402 or 429, stop this step, keep the accounts found so far and go on to step 7. Add one line under the table: "X accounts were checked for <n> of <total> people before the search stopped." `<n>` counts the people whose waterfall finished. Never retry a person with no account.

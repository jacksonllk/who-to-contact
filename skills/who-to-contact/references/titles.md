# Typed titles and seniority bands

Read this in step 3 of SKILL.md when the persona is `other` (the user typed job titles or an area with no list) or the user named a seniority band, and in step 4 when an area with no list finds no one.

## Clean the user's titles

1. Drop any term that starts with `-`, together with the `-`. This search does not support exclusions. Tell the user: "Left out <terms>: this search cannot exclude people."
2. Remove double quotes (straight and curly), parentheses and the words `OR` and `or` used between titles. Keep hyphens inside words, as in "Co-Founder".
3. Rewrite each item as a plain job title or function, in the user's language. Remove filler words: "the founders" becomes "Founder", "anyone in growth" becomes "Growth". Split on "and" only when it joins two full titles ("VP Growth and Head of Brand" becomes two). Keep one title whole when "and" joins two areas under it ("Head of Sales and Marketing").
4. Remove duplicates. Keep at most 6 titles. If the titles joined with ", " are longer than 80 characters, drop whole titles from the end until they fit. Tell the user in one line which titles were dropped.
5. Any word that names a role or a function counts as a title, for example "CEO" or "marketing". If nothing is left, ask once: "Which job titles do you want to reach at <company>? Separate them with commas." If the answer still has none, say "A job title is needed, so nothing was searched." and stop.

## Seniority bands

When the user names a band with a function ("VP-level marketing leaders", "director and above in finance"), keep only that persona's titles at the band and above, and use `other` with those titles.

The levels, from the top:
1. C-level: titles starting "Chief", plus CEO, President, Owner, Founder, Co-Founder and General Counsel.
2. VP.
3. Head.
4. Director, including Controller, HR Director and Design Director.

Manager titles are below Director. "VP-level" keeps C-level and VP titles. "Only VPs" keeps VP titles.

People search matches loosely, so it may still return people below the band. When no returned title is at the band or above, print before the table: "No one at <band> level matched. These are the closest matches."

## Fallback for an area with no list

When the search for an area with no list finds no one, run the persona that leads that area at most companies. The user gets the people closest to what they asked for. Match the area by meaning, not exact words.

| The user asked for | Fall back to |
|---|---|
| R&D, research, hardware, IT, infrastructure, platform, QA, DevOps | `engineering` |
| Manufacturing, supply chain, logistics, facilities, fulfillment | `operations` |
| Communications, PR, brand, content, community, events | `marketing` |
| Business development, alliances, channel | `partnerships` |
| Analytics, BI, data science, AI or ML research | `data` |
| Compliance, risk, privacy | `legal` |
| Talent, recruiting, culture | `people` |
| Strategy, corporate development, anything else | `founders` |

Use the persona's file name from the SKILL.md step 3 table. Its own fallback still applies after it (engineering falls back to founders).

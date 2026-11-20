# AGENTS.md

This repo is an agent skill: plain text files, no code to build or run.

- `skills/who-to-contact/SKILL.md` is the skill. Rarely used parts live in `skills/who-to-contact/references/`, and SKILL.md says when to read each one.
- Keep the frontmatter `description` in single quotes, and never put ": " inside it unquoted. After any change, `npx skills add ./ --list` must still list the skill.
- Never write `$` followed by a digit in SKILL.md, such as a price. Claude Code replaces `$0`, `$1` and similar with the words of the user's request. Write amounts as cents or US dollars.
- Read the Context.dev and Enrichable tool schemas before naming a tool or parameter. Never rely on remembered names.
- Every Context.dev call the skill makes carries the tag `who-to-contact`.
- Never add real people's names, emails or profiles to this repo, including in examples. Use invented people at example.com.

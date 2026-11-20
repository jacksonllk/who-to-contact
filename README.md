<div align="center">
  <h1>who-to-contact</h1>
  <p><strong>Find the right people at any company, with their LinkedIn, X and work emails, inside your AI agent.</strong><br>
  An open source agent skill for Claude Code, Codex and Cursor. Bring your own keys.</p>
  <p>
    <a href="#features">Features</a> ·
    <a href="#get-started">Get started</a> ·
    <a href="#usage">Usage</a> ·
    <a href="#build-on-it">Build on it</a>
  </p>

[![Built with context.dev](https://img.shields.io/badge/built%20with-context.dev-2563eb)](https://context.dev/?utm_source=who-to-contact&utm_medium=readme&utm_campaign=open-source)
[![MIT licence](https://img.shields.io/badge/licence-MIT-green)](LICENSE)

</div>

Ask your agent who to contact at a company. You get a table in the chat and a CSV file.

```
who are the engineering leaders at example.com?
```

| # | Name | Title | Work email |
|---|---|---|---|
| 1 | Jordan Lee | Chief Technology Officer | jordan@example.com |
| 2 | Priya Shah | VP of Engineering | priya.shah@example.com |
| 3 | Sam Ortiz | Director of Engineering | sam.ortiz@example.com |

*Illustrative example. The people and emails are invented.*

## Features

- **16 preset personas.** Engineering, sales, marketing, finance, security and more. Each has a job title list built from how people-data providers group roles. You can tweak and test different role combinations.
- **Any role you name.** Ask for exact titles, such as "Head of Marketing", or by a team's function such as R&D. If no one matches, the skill searches the team that usually leads that area.
- **Work emails with Enrichable.** Connect [Enrichable](https://www.enrichable.io/app), and every search also returns work emails.
- **X accounts on request.** Reply "X" after a search. The skill adds each person's X account when it can confirm one.
- **Lists of companies.** Upload a CSV of companies and get back a CSV of the people at each one.

## Get started

You need Claude Code, Codex or Cursor.

1. Create a free [Context.dev](https://context.dev/?utm_source=who-to-contact&utm_medium=readme&utm_campaign=open-source) account. It includes 1,000 free credits each month.
2. For work emails (optional), sign in to [Enrichable](https://www.enrichable.io/app) with your Context.dev account. Create an agent key under Configuration → Agent access.
3. Install the skill in your project:

```bash
npx skills add jacksonllk/who-to-contact
```

4. Connect your agent: **[Claude Code](#claude-code)** · **[Codex](#codex)** · **[Cursor](#cursor)** · **[Other agents](#other-agents)**

### Claude Code

Run these in your project folder. The second command opens the Context.dev sign-in page.

```bash
claude mcp add --transport http context https://mcp.context.dev/mcp
claude mcp login context
```

For work emails, also run this with your Enrichable agent key:

```bash
claude mcp add --transport http enrichable https://www.enrichable.io/mcp --header "Authorization: Bearer YOUR_AGENT_KEY"
```

Then restart Claude Code.

### Codex

Run this. It opens the Context.dev sign-in page.

```bash
codex mcp add context --url https://mcp.context.dev/mcp
```

For work emails, add `export ENRICHABLE_AGENT_KEY=YOUR_AGENT_KEY` to your shell profile, then run:

```bash
codex mcp add enrichable --url https://www.enrichable.io/mcp --bearer-token-env-var ENRICHABLE_AGENT_KEY
```

Then restart Codex.

### Cursor

Add this to `.cursor/mcp.json` in your project. Then open Customize → MCPs, find `context` and select Authenticate.

```json
{
  "mcpServers": {
    "context": { "url": "https://mcp.context.dev/mcp" }
  }
}
```

For work emails, use this file instead, with your Enrichable agent key:

```json
{
  "mcpServers": {
    "context": { "url": "https://mcp.context.dev/mcp" },
    "enrichable": {
      "url": "https://www.enrichable.io/mcp",
      "headers": { "Authorization": "Bearer YOUR_AGENT_KEY" }
    }
  }
}
```

### Other agents

Follow Context.dev's [MCP setup guide](https://docs.context.dev/install-mcp). For work emails, follow Enrichable's [MCP guide](https://github.com/context-dot-dev/enrichable/blob/main/docs/mcp.md). Your agent must support custom request headers.

### About work emails

Enrichable includes a one-time $1 trial. After that, add your own email provider key under Configuration → Connections. It supports Hunter, Apollo, Prospeo, ContactOut and QuickEnrich.

## Usage

```
who are the engineering leaders at example.com?
```

```
find the Head of Partner Marketing and VP Growth at example.com
```

```
find the engineering leaders at each company in this list
```

After a search, reply **X** for their X accounts, or **more** for 10 more people.

**Lists.** Drag a CSV into the chat or paste a list. Each row needs a company website or name. You get a new CSV with one row per person. Your own columns are kept.

**Files.** Results are saved in a `who-to-contact/` folder in your project. Git ignores it.

## Build on it

The skill is plain text. Edit these files to change it:

| To change | Edit |
|---|---|
| Job titles for each team | [`SKILL.md`](skills/who-to-contact/SKILL.md), step 3 |
| The fallback team for areas without a list | [`references/titles.md`](skills/who-to-contact/references/titles.md) |
| Lists of companies | [`references/list-mode.md`](skills/who-to-contact/references/list-mode.md) |
| Work emails | [`references/emails.md`](skills/who-to-contact/references/emails.md) |
| X accounts | [`references/x-step.md`](skills/who-to-contact/references/x-step.md) |

## Limitations

- People data can be missing or out of date. Check each person before you reach out.
- People are matched by the company's website, not its name. Give the website the company uses.
- X accounts are often not found.
- No phone numbers.
- The CSV holds personal data. Keep it out of public repos.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).

<div align="center">
  <h1>who-to-contact</h1>
  <p><strong>Find the right people at any company. Get their LinkedIn, X and work emails. All inside your AI agent.</strong><br>
  An open source AI agent skill to find leads. Bring your own keys.</p>
  <p>
    <a href="#features">Features</a> ·
    <a href="#get-started">Get started</a> ·
    <a href="#usage">Usage</a> ·
    <a href="#build-on-it">Build on it</a>
  </p>

[![Built with context.dev](https://img.shields.io/badge/built%20with-context.dev-2563eb)](https://context.dev/?utm_source=who-to-contact&utm_medium=readme&utm_campaign=open-source)
[![MIT licence](https://img.shields.io/badge/licence-MIT-green)](LICENSE)

</div>

Tell your agent who you are looking for at which company and let it get to work.

```
who are the engineering leaders at example.com? include their X accounts
```

| # | Name | Title | LinkedIn | X | Work email |
|---|---|---|---|---|---|
| 1 | Jordan Lee | Chief Technology Officer | `in/jordan-lee-example` | `@jordanlee_example` | jordan@example.com |
| 2 | Priya Shah | VP of Engineering | `in/priya-shah-example` | `@priyashah_example` | priya.shah@example.com |
| 3 | Sam Ortiz | Director of Engineering | `in/sam-ortiz-example` | – | sam.ortiz@example.com |

*Illustrative example. The people, profiles and emails are invented.*

## Features

- **16 preset personas.** Engineering, sales, marketing, finance, security and more. Each has a job title list built from how people-data providers group roles. You can tweak and test different role combinations.
- **Find any role.** Ask for exact titles, such as "Head of Marketing", or a team's function, such as R&D. If no one matches, the skill searches the team that usually leads that area.
- **Fetch work emails with Enrichable.** Connect the [Enrichable](https://www.enrichable.io/app) MCP to fetch work emails.
- **X accounts on request.** Reply "X" after a search and the skill will fetch each person's X account where available.
- **Lists of companies.** Upload a list of companies, tell it who to find in plain text and get back a list of the people.

## Get started

1. **Sign up** for a free [Context.dev account](https://www.context.dev/dashboard/api-keys). It includes 1,000 credits a month.
2. **Install the skill** in your project:

```bash
npx skills add jacksonllk/who-to-contact
```

3. **Connect the Context.dev MCP** to your agent: **[Claude Code](#claude-code)** · **[Codex](#codex)** · **[Cursor](#cursor)** · **[Other agents](#other-agents)**
4. **Optional, for work emails:** sign in to [Enrichable](https://www.enrichable.io/app) with the same Context.dev account, create an agent key under Configuration → Agent access, and connect the Enrichable MCP in the same steps. It includes some trial credits, after which you will need your own email provider key.

Now you can start prompting your agent to find leads.

### Claude Code

Run these in your project folder. The second command opens the sign-in page.

```bash
claude mcp add --transport http context https://mcp.context.dev/mcp
claude mcp login context
```

For work emails, also run this with your agent key:

```bash
claude mcp add --transport http enrichable https://www.enrichable.io/mcp --header "Authorization: Bearer YOUR_AGENT_KEY"
```

Then restart Claude Code.

### Codex

Run this. It opens the sign-in page.

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

For work emails, use this file instead, with your agent key:

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

## Usage

```
who are all the founders at example.com?
```

```
find the Head of Marketing at example.com
```

```
find the engineering team at each company in this list
```

After a search, reply **X** to fetch their X accounts, or reply **more** for 10 more people if there are more matches.

**Upload a list of companies.** Attach a CSV with each company's website and say who to find. You get back a new CSV with one row per person. Your original columns stay in it.

**Output file.** Results are saved in a `who-to-contact/` folder in your project. Git ignores it by default.

## Build on it

The skill is a series of plain-text instructions that your agent follows. You can change what it does by editing these files.

| If you want to change | Edit |
|---|---|
| The job titles searched for each persona | [`SKILL.md`](skills/who-to-contact/SKILL.md), the table in step 3 |
| Which team is searched when a role has no persona | [`references/titles.md`](skills/who-to-contact/references/titles.md) |
| How a list of companies is read and saved | [`references/list-mode.md`](skills/who-to-contact/references/list-mode.md) |
| How work emails are found and checked | [`references/emails.md`](skills/who-to-contact/references/emails.md) |
| How X accounts are found and checked | [`references/x-step.md`](skills/who-to-contact/references/x-step.md) |

## Limitations

- People data can sometimes be out of date. Check before outreach.
- Input the right company website for the best results.
- No phone numbers.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).

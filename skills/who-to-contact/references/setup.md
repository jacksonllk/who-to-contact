# Setup: connect Context.dev

Read this only when SKILL.md step 0 finds that `people-search` or `company-search` is not available. Make no other call. Follow the first row that fits.

| Situation | What you do |
|---|---|
| Claude Code, and `claude mcp list` shows a Context.dev connector (named `context`, or `claude.ai Context.dev`) as connected | The tools did not load in this session. Tell the user to restart Claude Code and ask again. Do not add a second server. |
| Claude Code, and `claude mcp list` shows a server named `context` that needs sign-in | Run `claude mcp login context`. It opens the browser. Tell the user to finish signing in, then restart Claude Code. |
| Claude Code, no Context.dev server | Ask: "I'll add the Context.dev server to this project and open the sign-in page. OK?" On yes, run `claude mcp add --transport http context https://mcp.context.dev/mcp`, then `claude mcp login context`. Ask first before adding `--scope user`, which adds it to every project. |
| The user has no Context.dev account | Tell them to create a free account at https://www.context.dev/dashboard/api-keys?utm_source=who-to-contact&utm_medium=skill&utm_campaign=open-source, then follow the row for their agent or app in this table. They do not need to copy a key. Sign-in creates the key the server uses. |
| Codex | Run `codex mcp get context`. If it shows the server (from `~/.codex/config.toml` or the project's `.codex/config.toml`), it only needs sign-in: run `codex mcp login context` and tell the user to finish in the browser, then restart Codex. If there is no server, ask first, because this adds it for every project: "I'll add the Context.dev server to Codex and open the sign-in page. OK?" On yes, run `codex mcp add context --url https://mcp.context.dev/mcp`. |
| Cursor | Ask first. On yes, create `.cursor/mcp.json` in the project with `{"mcpServers": {"context": {"url": "https://mcp.context.dev/mcp"}}}`. Tell the user to open Customize → MCPs (older versions: Cursor Settings → Tools & MCP), find `context` and select Authenticate. |
| Another agent (GitHub Copilot, Gemini CLI and others) | Show the setup steps for that agent from https://docs.context.dev/install-mcp. |
| claude.ai or the Claude app | Tell the user to open https://claude.ai/directory/context-dev, select Connect and sign in to Context.dev. Then start a new chat, open the + menu, choose Connectors and turn on Context.dev. Tell them to ask their question again in that chat. |
| ChatGPT | Tell the user to open https://chatgpt.com/plugins/plugin_asdk_app_6a70eca71e948191aea49dbb9b674477, install the Context.dev plugin and sign in. Then start a new chat and ask the question again there. |
| Any other app that can add a custom connector or remote MCP server | Tell the user to add a custom connector with the URL `https://mcp.context.dev/mcp` and sign in to Context.dev when asked. |

Then stop. Run the search on the user's next message.

## Connect Enrichable for work emails (optional)

Read this when the user asks how to get work emails or how to connect Enrichable. Enrichable (https://www.enrichable.io) finds work emails through a waterfall of email providers. Sign-in uses the user's Context.dev account. The agent key it gives is a secret: the user types it into their own terminal or config file. Never ask for it, read it back or print it.

Tell the user:
1. Sign in at https://www.enrichable.io/app with your Context.dev account.
2. Open Configuration → Agent access → Create agent key, and copy it.
3. Optional: under Configuration → Connections, add keys for email providers you already pay for (Hunter, Apollo, Prospeo, ContactOut, QuickEnrich). Without them, Enrichable's included allowance pays for lookups while it lasts. No email provider account is needed to start.
4. Connect it to your agent:

| Agent | What the user does |
|---|---|
| Claude Code | In the project folder, run `claude mcp add --scope local --transport http enrichable https://www.enrichable.io/mcp --header "Authorization: Bearer YOUR_AGENT_KEY"`, then restart Claude Code. |
| Other agents | Follow Enrichable's MCP guide at https://github.com/context-dot-dev/enrichable/blob/main/docs/mcp.md. The agent must support custom request headers. |
| The user runs their own Enrichable deployment | Same steps, with their deployment's address ending in `/mcp` instead of `https://www.enrichable.io/mcp`. Self-hosted deployments also need their OpenAI and Context.dev keys as headers, as Enrichable's MCP guide describes. |

Then stop. Work emails come with the next search.

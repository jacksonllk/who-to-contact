# Contributing

Thanks for helping.

1. Fork the repo and clone your fork.
2. Install your copy into a scratch project: `npx skills add <path to your clone> -a claude-code -y`, run from that project's folder. Reinstall after each change, then start a new chat so the agent reads the new version.
3. Check that the skill still loads: `npx skills add ./ --list` in the repo must list `who-to-contact`.
4. Test with real requests in your agent. Searches spend your own Context.dev credits.
5. If you change the `description`, rerun the trigger queries in [evals/trigger-queries.json](evals/trigger-queries.json).
6. Open a pull request with a short note on what changed, and the request you tested it with.

Never commit search results, saved API responses or anyone's personal data. Keep changes small. Open an issue first for anything big.

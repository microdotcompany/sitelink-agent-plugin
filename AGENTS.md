# AGENTS.md

This repo is the Sitelink plugin for ChatGPT, Codex and Claude: one skill plus an MCP config pointing at `https://mcp.site.link/mcp`, with a manifest for each. The MCP server lives in `microdotcompany/sitelink-mcp`.

## Layout

| Path                                | What it is                                                                            |
| ----------------------------------- | ------------------------------------------------------------------------------------- |
| `plugin.json`                       | Portable Agent Plugins manifest; OpenAI listing fields under `extensions.com.openai`  |
| `mcp.json`                          | MCP server (`streamable-http`, OAuth with DCR; `cimd` broke the OpenAI portal connect) |
| `.claude-plugin/plugin.json`        | Claude plugin manifest (claude.ai, Cowork, Claude Code)                               |
| `.mcp.json`                         | MCP server for Claude (`type: http`; Claude finds OAuth from the server itself)       |
| `skills/sitelink/SKILL.md`          | The workflow: check, claim, then publish the site or connect a ChatGPT Site, verify   |
| `skills/sitelink/agents/openai.yaml` | Skill interface and its MCP dependency                                               |
| `assets/`                           | The 512×512 listing and composer icon (OpenAI manifest only)                          |

## Rules

- The product is written **Sitelink** (one word, capital S, no dot) since 0.3.8 (2026-11-08), never "site.link" as a name. `site.link` stays only in addresses: URLs, `name.site.link`, emails.

- No secrets, hooks, commands, agents or `userConfig`. OpenAI's directory rejects them.
- No `.DS_Store` files, symlinks or submodules.
- The tools describe themselves. The skill teaches the workflow across the Sitelink tools and the ChatGPT Sites tools, not tool parameters. When a tool changes in `sitelink-mcp`, check the recipes and the error table still match.
- Keep `SKILL.md` under 300 lines.
- Bump `version` in `plugin.json`, `.claude-plugin/plugin.json` and `metadata.version` in `SKILL.md` together. CI checks they match.
- The skill is shared by every app. Keep it app-neutral: it names the ChatGPT Sites tools only in the ChatGPT Sites flow.
- Changing the manifest, skill or MCP config needs a new ZIP and another OpenAI review, and a new submission in Claude's developer portal (claude.ai/directory/manage). Tool changes on the server go live after OpenAI's automatic rescan.
- Run `claude plugin validate .` before submitting to Claude.
- OpenAI's skill scan ("could create a security risk") gives different answers for the same text, so keep the skill well clear of what it flags: running commands taken from tool output, acting "without asking", relaying plans or prices, and searching other apps' data. It flagged 0.3.0 (plan relay) and 0.3.3, and passed 0.3.5.
- 0.3.5 is live in the ChatGPT directory: this skill without the two new-chat `@` lines (`@site.link` until 0.3.8, `@sitelink` since) (in "Before you start" and step 1 of "Connect a ChatGPT Site"). 0.3.7 is the first OpenAI ZIP with them. If the skill scan flags it, drop those two lines (and their mention in the release notes) and upload again; the scan runs before submit, so this costs no review time.
- The directory search ranks names that contain the searched word first ("website" found 48 plugins and not ours while its name was only "site.link"). That's why `displayName` carries the word: "Sitelink for Websites" since 0.3.9 (the user picked it over "Website Publisher", 2026-11-08). Keep "Sitelink" at the start so typing `@sitelink` in a chat still finds it, and keep `shortDescription` at 30 characters or fewer, like the other cards.
- OpenAI reads the review test cases (exactly 5 positive, 3 negative), the demo video and the release notes from `extensions.com.openai.review` and `publication` in `plugin.json`, and shows them read-only in the dashboard. Change them here and upload a new ZIP.
- Reviewer credentials never go in the ZIP; OpenAI rejects `test_credentials`. They're in the dashboard's review form: "Username and password", reviewer@site.link, with the password typed by a person. The reviewer account must sign in without an emailed code: 0.3.5 was rejected when reviewers took the old "Check your email" page as a verification step, so mcp.site.link now asks the review login for a password.
- Every new draft has to reconnect the MCP on the MCPs tab and sign in as the reviewer before it can be submitted.

## Package for submission

From a clean checkout of the version you're submitting. AGENTS.md stays out of the ZIP: it's notes for us, not the reviewer.

```
git archive --format=zip -o ../sitelink-plugin.zip HEAD -- . ':!.github' ':!.claude-plugin' ':!.mcp.json' ':!AGENTS.md'
```

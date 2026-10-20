# AGENTS.md

This repo is the site.link plugin for ChatGPT, Codex and Claude: one skill plus an MCP config pointing at `https://mcp.site.link/mcp`, with a manifest for each. The MCP server lives in `microdotcompany/sitelink-mcp`.

## Layout

| Path                                | What it is                                                                            |
| ----------------------------------- | ------------------------------------------------------------------------------------- |
| `plugin.json`                       | Portable Agent Plugins manifest; OpenAI listing fields under `extensions.com.openai`  |
| `mcp.json`                          | MCP server (`streamable-http`, OAuth; no `client` block, the portal rejects `cimd`)  |
| `.claude-plugin/plugin.json`        | Claude plugin manifest (claude.ai, Cowork, Claude Code)                               |
| `.mcp.json`                         | MCP server for Claude (`type: http`; Claude finds OAuth from the server itself)       |
| `skills/sitelink/SKILL.md`          | The workflow: check, claim, then publish the site or connect a ChatGPT Site, verify   |
| `skills/sitelink/agents/openai.yaml` | Skill interface and its MCP dependency                                               |
| `assets/icon.png`                   | 512×512 listing and composer icon                                                     |

## Rules

- No secrets, hooks, commands, agents or `userConfig`. OpenAI's directory rejects them.
- No `.DS_Store` files, symlinks or submodules.
- The tools describe themselves. The skill teaches the workflow across the site.link tools and the ChatGPT Sites tools, not tool parameters. When a tool changes in `sitelink-mcp`, check the recipes and the error table still match.
- Keep `SKILL.md` under 300 lines.
- Bump `version` in `plugin.json`, `.claude-plugin/plugin.json` and `metadata.version` in `SKILL.md` together. CI checks they match.
- The skill is shared by every app. Keep it app-neutral: it names the ChatGPT Sites tools only in the ChatGPT Sites flow.
- Changing the manifest, skill or MCP config needs a new ZIP and another OpenAI review, and a new submission in Claude's developer portal (claude.ai/directory/manage). Tool changes on the server go live after OpenAI's automatic rescan.
- Run `claude plugin validate .` before submitting to Claude.

## Package for submission

```
zip -r ../sitelink-plugin.zip . -x '.git/*' '.github/*' '.claude-plugin/*' '.mcp.json' '*.zip'
```

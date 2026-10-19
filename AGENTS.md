# AGENTS.md

This repo is the site.link plugin for ChatGPT (and Codex): one skill plus an MCP config pointing at `https://mcp.site.link/mcp`. The MCP server lives in `microdotcompany/sitelink-mcp`.

## Layout

| Path                                | What it is                                                                            |
| ----------------------------------- | ------------------------------------------------------------------------------------- |
| `plugin.json`                       | Portable Agent Plugins manifest; OpenAI listing fields under `extensions.com.openai`  |
| `mcp.json`                          | MCP server (`streamable-http`, OAuth with CIMD)                                       |
| `skills/sitelink/SKILL.md`          | The workflow: check, claim, add the domain on the Site, connect, verify               |
| `skills/sitelink/agents/openai.yaml` | Skill interface and its MCP dependency                                               |
| `assets/icon.png`                   | 512×512 listing and composer icon                                                     |

## Rules

- No secrets, hooks, commands, agents or `userConfig`. OpenAI's directory rejects them.
- No `.DS_Store` files, symlinks or submodules.
- The tools describe themselves. The skill teaches the workflow across the site.link tools and the ChatGPT Sites tools, not tool parameters. When a tool changes in `sitelink-mcp`, check the recipes and the error table still match.
- Keep `SKILL.md` under 300 lines.
- Bump `version` in `plugin.json` and `metadata.version` in `SKILL.md` together. CI checks they match.
- Changing the manifest, skill or MCP config needs a new ZIP and another OpenAI review. Tool changes on the server go live after OpenAI's automatic rescan.

## Package for submission

```
zip -r ../sitelink-plugin.zip . -x '.git/*' '.github/*' '*.zip'
```

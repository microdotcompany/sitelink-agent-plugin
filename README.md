# site.link for ChatGPT and Claude

Put the website you make with ChatGPT or Claude online at a free address like `mycafe.site.link`, without buying a domain or touching DNS.

Ask:

> Publish this page at a site.link address

site.link reserves the name and hosts the site. It's live in seconds, and publishing again updates it. If you built the site with ChatGPT Sites, site.link can point the name at that Site instead: ChatGPT adds it as a custom domain and site.link creates the DNS records Sites asks for.

Each account gets one free name. Sign in with email or Google the first time you're asked.

## What's in here

The site.link MCP connector (`https://mcp.site.link/mcp`) and a skill that walks the assistant through checking and claiming a name, publishing the site or connecting a ChatGPT Site, and checking the address works. The server code is in [`sitelink-mcp`](https://github.com/microdotcompany/sitelink-mcp).

- **ChatGPT and Codex** read `plugin.json` and `mcp.json`.
- **Claude** (claude.ai, Cowork, Claude Code) reads `.claude-plugin/plugin.json` and `.mcp.json`.

## Data

The plugin sends the name you pick, your site's files, and for ChatGPT Sites the DNS records Sites shows, to site.link through mcp.site.link. Hosted sites are public at their address. See https://site.link/privacy.

Support: care@site.link

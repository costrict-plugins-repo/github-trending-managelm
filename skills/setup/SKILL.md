---
name: setup
description: Help the user configure their ManageLM connection to Claude Code. Use when the user asks to set up, configure, or connect ManageLM.
disable-model-invocation: true
---

# ManageLM Setup

Guide the user through connecting Claude to their ManageLM portal:

1. **Get the portal URL**: Ask the user for their ManageLM portal URL (e.g., `https://app.managelm.com`).

2. **Configure the MCP endpoint**: The MCP endpoint is `{portal_url}/mcp` — the URL **must** end with `/mcp`.
   - **With this plugin installed** (the usual case, since this skill ships with it), the `managelm` server is already configured and points to `https://app.managelm.com/mcp`. For a self-hosted portal, do NOT add a second server: have the user set `MANAGELM_MCP_URL` (e.g. `export MANAGELM_MCP_URL=https://portal.example.com/mcp`) in the environment Claude Code starts from, then restart Claude Code.
   - **Without the plugin**, add the server with `claude mcp add --transport http managelm {portal_url}/mcp`, or in a project `.mcp.json`:
   ```json
   {
     "mcpServers": {
       "managelm": {
         "type": "http",
         "url": "https://app.managelm.com/mcp"
       }
     }
   }
   ```

3. **Authentication**: ManageLM uses OAuth 2.0 with PKCE and Dynamic Client Registration. On first connection, Claude registers itself and opens a browser window for the user to sign in and authorize access. Nothing needs to be generated first: a ManageLM account is enough.

4. **Verify the connection**: After setup, use the `search_agents` tool to verify the connection works and show available servers.

## Troubleshooting

- **401 Unauthorized**: The sign-in did not complete or the token was revoked (for example after the user reset their MCP credentials in Settings > MCP & API). Run `/mcp`, select the ManageLM server (listed as `plugin:managelm:managelm` when it comes from the plugin) and authenticate again.
- **Connection refused**: Verify the portal URL is correct, accessible from this machine, and ends with `/mcp`.
- **No agents listed**: The user may not have any agents enrolled, or their account role may restrict agent visibility.
- **Token expired**: Access tokens expire after 30 minutes and are automatically refreshed. If issues persist, re-authenticate: run `/mcp`, select the ManageLM server and authenticate again.

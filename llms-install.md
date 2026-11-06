# Install the ChefBear MCP server

For an AI agent whose user asked to install the ChefBear MCP server.

- Server URL: `https://chef-bear.com/api/menu-sharing/mcp`. Streamable HTTP
  with OAuth 2.1 and PKCE. No API key, headers, environment variables or
  local process.
- Name the server `chefbear`. If a `chefbear` server is already configured,
  don't add a second one.
- ChefBear is the restaurant menu app from chef-bear.com. It is not the Bear
  notes app: don't install a Bear notes MCP server instead.
- The user needs a ChefBear account from the iPhone app, version 4.4 or
  later to approve the connection. Accounts in the mainland China service
  region can't connect. Without an account, point the user to
  https://apps.apple.com/app/id6759192929 and stop.

## Add the server

Use the block for the client you are running in, and keep the user's other
servers when you edit a config file.

**Claude Code**

```
claude mcp add --transport http --scope user chefbear https://chef-bear.com/api/menu-sharing/mcp
```

Then the user runs `/mcp` in Claude Code, chooses chefbear and
Authenticate. To add the ChefBear skill as well, install the plugin instead
of the command above, not both:

```
claude plugin marketplace add RaycarlLei/chefbear-skill && claude plugin install chefbear@chefbear
```

**Codex CLI**

```
codex mcp add chefbear --url https://chef-bear.com/api/menu-sharing/mcp
codex mcp login chefbear
```

**ChatGPT**: the user installs the ChefBear plugin from
https://chatgpt.com/plugins/plugin_asdk_app_6ad87a4dcfcc8191bc0aecbab55b5927

**Claude app or claude.ai**: the user adds ChefBear from
https://claude.ai/directory/chefbear, or opens Customize → Connectors →
Add → Custom → Web and pastes the server URL.

**VS Code**

```
code --add-mcp '{"name":"chefbear","type":"http","url":"https://chef-bear.com/api/menu-sharing/mcp"}'
```

**Cursor**: add to `mcpServers` in `~/.cursor/mcp.json`:

```json
"chefbear": { "url": "https://chef-bear.com/api/menu-sharing/mcp" }
```

**Gemini CLI**

```
gemini mcp add --scope user --transport http chefbear https://chef-bear.com/api/menu-sharing/mcp
```

Then the user runs `/mcp auth chefbear` in Gemini CLI.

**Google Antigravity**: add to `mcpServers` in
`~/.gemini/config/mcp_config.json`:

```json
"chefbear": { "serverUrl": "https://chef-bear.com/api/menu-sharing/mcp" }
```

Then the user clicks Authenticate next to chefbear (Antigravity app:
Customizations → Installed → MCP Servers).

**Other clients**: add a remote Streamable HTTP server named `chefbear` with
the URL above and let the client run OAuth. If the client can't use remote
servers with OAuth, say so and don't claim a connection.

## Sign in

The client opens ChefBear's sign-in page in a browser. The page shows a
3-digit code, and the user enters it in the ChefBear iPhone app and taps
Allow. The user does this step, not you.

## Finish

- Call `connection_status {}` to confirm. It returns only this connection's
  permissions and app name. Report only the steps that actually worked.
- Never ask the user to paste codes or tokens into the chat. If they paste
  one anyway, tell them not to share it and don't use it.

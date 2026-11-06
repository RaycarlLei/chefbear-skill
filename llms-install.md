# Install the ChefBear MCP server

For an AI agent whose user asked to install the ChefBear MCP server.

- Server URL: `https://chef-bear.com/api/menu-sharing/mcp`. Streamable HTTP
  with OAuth 2.1 and PKCE. No API key, headers, environment variables or
  local process.
- Name the server `chefbear`. If a `chefbear` server is already configured,
  don't add a second one.
- ChefBear is the restaurant menu app from chef-bear.com. It is not the Bear
  notes app: don't install a Bear notes MCP server instead.
- The user needs a ChefBear account and the ChefBear iPhone app, version 4.4
  or later, to approve the connection. Accounts in the mainland China service
  region can't connect. Without an account, point the user to
  https://apps.apple.com/app/id6759192929 and stop.

## Add the server

Use the block for the client you are running in, and keep the user's other
servers when you edit a config file.

**Claude Code**

```
claude mcp add --transport http --scope user chefbear https://chef-bear.com/api/menu-sharing/mcp
```

Then the user starts a new Claude Code session, runs `/mcp`, picks chefbear
(plugin:chefbear:chefbear when installed by the plugin) and chooses
Authenticate. Newer Claude Code versions can also sign in with
`claude mcp login chefbear`; this is optional.

To add the ChefBear skill as well, install the plugin instead of the command
above, not both:

```
claude plugin marketplace add RaycarlLei/chefbear-skill && claude plugin install chefbear@chefbear
```

After installing the plugin, the user starts a new Claude Code session, runs
`/mcp`, picks chefbear (plugin:chefbear:chefbear when installed by the
plugin) and chooses Authenticate.

**Codex CLI**

```
codex mcp add chefbear --url https://chef-bear.com/api/menu-sharing/mcp
codex mcp login chefbear
```

**ChatGPT**: the user installs the ChefBear plugin from
https://chatgpt.com/plugins/plugin_asdk_app_6ad87a4dcfcc8191bc0aecbab55b5927

**Claude app or claude.ai**: the user adds ChefBear from
https://claude.ai/directory/chefbear. To add it by URL on Pro or Max, the
user opens Customize → Connectors, clicks + Add, then Add custom connector,
and pastes the server URL (on Team and Enterprise, an Owner adds it under
Organization settings → Connectors). For the skill as well, the user adds
the plugin marketplace `RaycarlLei/chefbear-skill` (Customize → Plugins →
Add → Add marketplace → Add from a repository) and installs ChefBear
instead of the directory connector, so there aren't two ChefBear connectors.
Plugins need a paid Claude plan (https://support.claude.com/en/articles/13837440)
and work on the web and in Claude Desktop; on the Free plan, the user adds
the directory connector instead.

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

Then the user runs `/mcp auth chefbear` in Gemini CLI. Alternatively,
install the Gemini CLI extension instead of the command above, not both:
`gemini extensions install https://github.com/RaycarlLei/chefbear-skill`.

**Google Antigravity**: add this to the `mcpServers` config, keeping the
servers already there, and save:

```json
{"mcpServers":{"chefbear":{"serverUrl":"https://chef-bear.com/api/menu-sharing/mcp"}}}
```

- Antigravity IDE (version 2.6.0 or later; ChefBear accepts Antigravity
  only through its client metadata document): agent panel … → MCP Servers →
  Manage MCP Servers → View raw config.
- Antigravity App: Customizations → Installed → MCP Servers → Copy Config
  File Path, then add the same entry to that file.

Then the user chooses Authenticate for chefbear (App: Customizations →
Installed → MCP Servers → Authenticate) and finishes signing in to ChefBear
in the browser. Antigravity's page then shows a code, which the user pastes
into Antigravity's own "Enter authorization code" field and submits. That
paste is expected: the code is not ChefBear's 3-digit code, and it never
goes into a chat.

**Other clients**: other MCP clients that sign in with OAuth and return to a
localhost callback can add a remote Streamable HTTP server named `chefbear`
with the URL above and let the client run OAuth. If the client can't add
it, or can't finish signing in to ChefBear, say so, don't claim a
connection, and suggest Claude, Claude Code or ChatGPT instead.

## Sign in

The client opens ChefBear's sign-in page in a browser. The page shows a
3-digit code, and the user enters it in the ChefBear iPhone app and taps
Allow. The user does this step, not you.

## Finish

- Call `connection_status {}` to confirm. It returns only this connection's
  permissions and app name. Report only the steps that actually worked.
- Never ask the user to paste codes or tokens into the chat. If they paste
  one anyway, tell them not to share it and don't use it. (Pasting
  Antigravity's authorization code into Antigravity's own field is part of
  its sign-in, not a chat.)

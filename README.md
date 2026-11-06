# ChefBear MCP server

The official MCP server for [ChefBear](https://www.chef-bear.com/), the
iPhone app that turns restaurant menus into structured, translated menus and
helps you decide what to order.

```
https://chef-bear.com/api/menu-sharing/mcp
```

- **Transport:** Streamable HTTP with OAuth 2.1 and PKCE. Clients sign in
  with a Client ID Metadata Document or dynamic client registration. There is
  no API key.
- **Who can connect:** anyone with a ChefBear account. Get the free
  [ChefBear iPhone app](https://apps.apple.com/app/id6759192929) and sign in.
  You approve each connection in the app (version 4.4 or later) by entering
  the 3-digit code that ChefBear's sign-in page shows. Accounts in the
  mainland China service region can't connect.
- **Not the Bear notes app.** Search results for "Bear MCP" are mostly
  servers for the Bear note-taking app. They have nothing to do with
  ChefBear.

This repository holds the server's public listing files, the open-source
`chefbear` Agent Skill and the ChefBear plugin for Claude (MIT).

## Install

### Claude (claude.ai, desktop and mobile)

Add ChefBear from the [Claude directory](https://claude.ai/directory/chefbear),
or open **Customize → Connectors → Add → Custom → Web** and paste the server
URL.

To get the skill as well, add this repository as a plugin marketplace:
**Customize → Plugins → Add → Add marketplace**, enter
`RaycarlLei/chefbear-skill`, then install **ChefBear**.

### Claude Code

```
claude mcp add --transport http --scope user chefbear https://chef-bear.com/api/menu-sharing/mcp
```

Then run `/mcp` in Claude Code, choose **chefbear** and **Authenticate**.
Recent versions can also sign in from the shell with
`claude mcp login chefbear`.

For the server and the skill together, install the plugin instead:

```
claude plugin marketplace add RaycarlLei/chefbear-skill
claude plugin install chefbear@chefbear
```

### ChatGPT and Codex

Install the [ChefBear plugin](https://chatgpt.com/plugins/plugin_asdk_app_6ad87a4dcfcc8191bc0aecbab55b5927)
from the plugin directory. To add the server to the Codex CLI by hand:

```
codex mcp add chefbear --url https://chef-bear.com/api/menu-sharing/mcp
codex mcp login chefbear
```

### VS Code

[Install in VS Code](https://vscode.dev/redirect?url=vscode%3Amcp%2Finstall%3F%257B%2522name%2522%253A%2522chefbear%2522%252C%2522type%2522%253A%2522http%2522%252C%2522url%2522%253A%2522https%253A%252F%252Fchef-bear.com%252Fapi%252Fmenu-sharing%252Fmcp%2522%257D),
or run:

```
code --add-mcp '{"name":"chefbear","type":"http","url":"https://chef-bear.com/api/menu-sharing/mcp"}'
```

### Cursor

[Add to Cursor](https://cursor.com/install-mcp?name=chefbear&config=eyJ1cmwiOiJodHRwczovL2NoZWYtYmVhci5jb20vYXBpL21lbnUtc2hhcmluZy9tY3AifQ%3D%3D),
or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "chefbear": { "url": "https://chef-bear.com/api/menu-sharing/mcp" }
  }
}
```

### Gemini CLI

```
gemini mcp add --scope user --transport http chefbear https://chef-bear.com/api/menu-sharing/mcp
```

Then run `/mcp auth chefbear` in Gemini CLI. You can also install this
repository as an extension:
`gemini extensions install https://github.com/RaycarlLei/chefbear-skill`.

### Google Antigravity

Add this to `~/.gemini/config/mcp_config.json`, or in the agent panel open
**… → MCP Servers → Manage MCP Servers → View raw config**:

```json
{
  "mcpServers": {
    "chefbear": { "serverUrl": "https://chef-bear.com/api/menu-sharing/mcp" }
  }
}
```

In the Antigravity app, open **Customizations → Installed** and click
**Authenticate** next to chefbear under **MCP Servers**.

### Other MCP clients

Any client that supports remote Streamable HTTP servers with OAuth can add
the server URL and sign in. If ChefBear's sign-in page turns your app away,
use one of the clients above.

### Check the connection

After you approve in the ChefBear app, ask your assistant to call
`connection_status`. It returns this connection's permissions and app name,
with no account or menu data.

## Tools

| Tool | What it does |
|---|---|
| `import_menu` | Creates a private menu from 1 to 5 menu photos (public `https://` links, or uploaded files where the app supports them). Uses one recognition from your allowance. |
| `get_import_status` | Says whether an import is pending, ready (with the menu ID and a private link) or failed. |
| `save_menu` | Saves dishes the assistant has already read, for example from a photo in the chat, as a private menu at once. No recognition, no allowance. |
| `list_menus` | Lists your saved menus, newest first, so the assistant can find the one you mean. |
| `get_menu` | Reads a saved menu: dish names, short descriptions, categories and prices. |
| `get_shared_menu` | Reads what a `https://chef-bear.com/m/` share link shows. |
| `share_menu` | Turns on a public link for a menu. Anyone with the link sees the dishes and prices, not the photos. |
| `revoke_menu_share` | Turns a public link off. The private menu stays. |
| `get_taste_profile` | Reads the likes, dislikes, allergies, diets and taste levels you saved in ChefBear. |
| `update_taste_profile` | Adds or removes what you ask it to remember about your tastes, and records dishes you picked or turned down. |
| `recommend_dishes` | Ranks a saved menu's dishes against your taste profile and what you said, and lists dishes whose menu text mentions an allergy or dislike as excluded. |
| `connection_status` | Shows this connection's permissions and app name. |

`initialize` and `tools/list` work without signing in; every tool call needs
a signed-in connection. Listing menus and the taste profile tools need their
own permissions, which you approve separately and which ChefBear may not
offer to every app. ChefBear doesn't find, book or order from restaurants,
take payments or give recipes.

## Agent Skill and plugin

The `chefbear` skill tells an agent when to help someone choose from a menu
and how to use the tools: picks only from the actual menu, careful wording
about allergies and diets, sharing only on request, and the right links back
to ChefBear. It helps even before you connect, from a menu photo or text in
the chat.

| Path | What it is |
|---|---|
| `server.json` | MCP Registry entry `com.chef-bear/chefbear` |
| `llms-install.md` | Install steps for AI agents |
| `gemini-extension.json` | Gemini CLI extension |
| `.claude-plugin/marketplace.json` | Claude plugin marketplace `chefbear` |
| `plugins/chefbear/.mcp.json` | The server, for the Claude plugin |
| `plugins/chefbear/skills/chefbear/SKILL.md` | The skill |
| `plugins/chefbear/skills/chefbear/references/tools.md` | Tools, permissions, errors and links |
| `INSTALL_PROMPT.md` | A prompt that installs the Claude plugin for you |

## Privacy and safety

- You approve every connection in the ChefBear app. No password, code or
  token goes into a chat. Disconnect anytime in your AI app, or in ChefBear
  under **Settings → Connected apps**.
- A connection reaches your ChefBear menus and, only if you allow it, your
  taste profile. It can't place orders or take payments.
- Dish details come from the restaurant's menu. Confirm allergens and
  ingredients with the restaurant.

[Privacy policy](https://www.chef-bear.com/privacy/) ·
[Terms](https://www.chef-bear.com/terms/) ·
[Support](https://www.chef-bear.com/support)

## Links

- [ChefBear MCP server page](https://www.chef-bear.com/mcp/)
- [Setup guide for AI assistants](https://www.chef-bear.com/ai/)
  ([machine-readable](https://www.chef-bear.com/ai/setup.json))
- [ChefBear in the Claude directory](https://claude.ai/directory/chefbear)
- [ChefBear plugin for ChatGPT and Codex](https://chatgpt.com/plugins/plugin_asdk_app_6ad87a4dcfcc8191bc0aecbab55b5927)
- [ChefBear on the App Store](https://apps.apple.com/app/id6759192929)

MIT licensed.

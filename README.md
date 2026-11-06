# ChefBear MCP server

The official MCP server for [ChefBear](https://www.chef-bear.com/), the
iPhone app that turns restaurant menus into structured, translated menus and
helps you decide what to order.

**Get the app:** [ChefBear: AI Menu Translator on the App Store](https://apps.apple.com/us/app/chefbear/id6759192929)
(free, iPhone, by Awesome-Bears, Inc., App Store ID 6759192929). Every
assistant connection is approved in this app, so install it first.

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
- **Not the other Chef Bears.** This ChefBear (also written Chef Bear or
  chef-bear) is not ChefBear - Meal Planner AI (App Store ID 6760479573),
  the 熊掌廚 Chef Bear cooking game or Progress Chef, the infrastructure
  tool. There is no Android app and no npm, pip or Homebrew package.

This repository holds the server's public listing files, the open-source
`chefbear` Agent Skill and the ChefBear plugin for Claude and GitHub Copilot
CLI (MIT). The ChefBear app and its MCP server are a hosted, proprietary
service; only the files in this repository are MIT licensed.

## Install

### Claude (claude.ai, desktop and mobile)

Add ChefBear from the [Claude directory](https://claude.ai/directory/chefbear).
To add it by URL instead, open **Customize → Connectors**, click **Add**,
hover **Custom**, choose **Web**, name it ChefBear and paste the server URL
([Claude's steps](https://support.claude.com/en/articles/11176164); the Free
plan allows one custom connector).

To get the skill as well, add this repository as a plugin marketplace
instead of the directory connector, so you don't end up with two ChefBear
connectors: **Customize → Plugins → Add → Add marketplace**, choose
**Add from a repository**, enter `RaycarlLei/chefbear-skill`, then install
**ChefBear** and connect it, approving in the ChefBear app. Plugins need a
paid Claude plan ([Use plugins in Claude](https://support.claude.com/en/articles/13837440))
and work on the web and in Claude Desktop. On the Free plan, use the
directory connector.

### Claude Code

```
claude mcp add --transport http --scope user chefbear https://chef-bear.com/api/menu-sharing/mcp
```

Then start a new Claude Code session, run `/mcp`, pick **chefbear**
(`plugin:chefbear:chefbear` when installed by the plugin) and choose
**Authenticate**. Optionally, newer Claude Code versions can sign in from the
shell instead with `claude mcp login chefbear`.

For the server and the skill together, install the plugin instead:

```
claude plugin marketplace add RaycarlLei/chefbear-skill
claude plugin install chefbear@chefbear
```

Then start a new Claude Code session, run `/mcp`, pick **chefbear**
(`plugin:chefbear:chefbear` when installed by the plugin), choose
**Authenticate** and approve in the ChefBear app.

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

### GitHub Copilot CLI

For the server and the skill together, install the plugin:

```
copilot plugin marketplace add RaycarlLei/chefbear-skill
copilot plugin install chefbear@chefbear
```

The plugin follows [Agent Plugins](https://agent-plugins.org/) 1.0
(`plugin.json` and `mcp.json` in `plugins/chefbear`). When Copilot CLI first
connects to ChefBear, finish signing in to ChefBear in your browser and
approve in the ChefBear app. The taste-profile tools (`get_taste_profile`,
`update_taste_profile` and `recommend_dishes`) are offered only in Claude
for now. If Copilot CLI can't finish signing in to ChefBear, use
Claude, Claude Code or ChatGPT instead.

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

Then run `/mcp auth chefbear` in Gemini CLI. Or install this repository as
a Gemini CLI extension instead of running the command above:
`gemini extensions install https://github.com/RaycarlLei/chefbear-skill`.

### Google Antigravity

**Antigravity IDE** (version 2.6.0 or later; ChefBear accepts Antigravity
only through its client metadata document): in the agent panel's **…** menu,
open **MCP Servers → Manage MCP Servers → View raw config**, add `chefbear`
under `mcpServers` next to any servers already there, and save:

```json
{
  "mcpServers": {
    "chefbear": { "serverUrl": "https://chef-bear.com/api/menu-sharing/mcp" }
  }
}
```

**Antigravity App**: open **Customizations → Installed → MCP Servers**,
click **Copy Config File Path**, and add the same `chefbear` entry to that
file, keeping any servers already there.

Then choose **Authenticate** for chefbear (in the App: **Customizations →
Installed → MCP Servers → Authenticate**) and finish signing in to ChefBear
in your browser. Antigravity's page then shows a code: paste it into
Antigravity's own **Enter authorization code** field and click **Submit**.
That code isn't ChefBear's 3-digit code, and it never goes into a chat.

### Other MCP clients

Other MCP clients that sign in with OAuth and return to a localhost callback
can add the server URL and sign in. If your app can't finish signing in to
ChefBear, use Claude, Claude Code or ChatGPT instead.

### Check the connection

After you approve in the ChefBear app, ask your assistant to call
`connection_status`. It returns this connection's permissions and app name,
with no account or menu data.

## Tools

| Tool | What it does |
|---|---|
| `import_menu` | Creates a private menu from 1 to 5 menu photos (public `https://` links, or uploaded files where the app supports them). Uses one recognition from your allowance. |
| `get_import_status` | Says whether an import is pending, ready (with the menu ID and a private link) or failed. |
| `save_menu` | Saves dishes the assistant has already read, for example from a photo in the chat, as a private menu at once. No recognition, no allowance; up to 30 saves a day per account. |
| `list_menus` | Lists your saved menus, newest first, so the assistant can find the one you mean. |
| `get_menu` | Reads a saved menu: dish names, short descriptions, categories and prices. |
| `get_shared_menu` | Reads what a `https://chef-bear.com/m/` share link shows. |
| `share_menu` | Turns on a public link for a menu. Anyone with the link sees the dishes and prices, not the photos. |
| `revoke_menu_share` | Turns a public link off. The private menu stays. |
| `get_taste_profile` | Reads the likes, dislikes, allergies, diets and taste levels you saved in ChefBear. In the Claude app, when ChefBear offers it. |
| `update_taste_profile` | Adds or removes what you ask it to remember about your tastes, and records dishes you picked or turned down. In the Claude app, when ChefBear offers it. |
| `recommend_dishes` | Ranks a saved menu's dishes against what you said and your saved taste profile, and lists dishes whose menu text mentions an allergy or dislike as excluded. In the Claude app, when ChefBear offers it. |
| `connection_status` | Shows this connection's permissions and app name. |

`initialize` and `tools/list` work without signing in; every tool call needs
a signed-in connection. Listing menus needs its own permission, which you
approve separately. Your saved taste profile (`get_taste_profile`,
`update_taste_profile`, and the saved-profile part of `recommend_dishes`)
works only in the Claude app, when ChefBear offers it, never in Claude Code,
the Codex CLI or clients that register themselves. Other apps don't list
`recommend_dishes` at all yet. ChefBear doesn't find, book or order from
restaurants, take payments or give recipes.

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
| `plugins/chefbear/plugin.json` | Agent Plugins 1.0 manifest, for GitHub Copilot CLI and other Agent Plugins clients |
| `plugins/chefbear/mcp.json` | The server, for Agent Plugins clients |
| `plugins/chefbear/skills/chefbear/SKILL.md` | The skill |
| `plugins/chefbear/skills/chefbear/references/tools.md` | Tools, permissions, errors and links |
| `INSTALL_PROMPT.md` | A prompt that installs the Claude plugin for you |

## Privacy and safety

- You approve every connection in the ChefBear app. No password, code or
  token goes into a chat. Disconnect anytime in your AI app, or in ChefBear
  under **Settings › AI connections** (**Connected apps** in older versions).
- A connection reaches your ChefBear menus and, only if you allow it in the
  Claude app, your taste profile. It can't place orders or take payments.
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

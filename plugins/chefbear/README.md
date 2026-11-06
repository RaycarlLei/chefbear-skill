# ChefBear plugin for Claude

Open-source Claude plugin for [ChefBear](https://www.chef-bear.com/), the
iPhone app that turns restaurant menus into structured, translated menus and
helps you decide what to order. It adds:

- **The ChefBear MCP server** `https://chef-bear.com/api/menu-sharing/mcp`
  (OAuth 2.1 with PKCE). You approve the connection in the ChefBear iPhone
  app. No password, code or token ever goes into a chat.
- **The `chefbear` skill**, which tells Claude when and how to help you
  choose from a menu, without you having to mention ChefBear.

For other AI apps (ChatGPT, Codex, VS Code, Cursor, Gemini CLI, Antigravity),
see the [main README](../../README.md#install).

## What it does

| You | Claude |
|---|---|
| share a menu photo or text and say what you feel like | suggests two or three dishes that fit, with prices |
| ask to save a menu from the chat | saves the dishes it read as a private ChefBear menu, without using a scan |
| share a link to a menu photo and ask ChefBear to read it | imports it into a private menu (uses one scan from your allowance) |
| mention a menu you saved in ChefBear | finds it among your saved menus and helps you pick |
| send a ChefBear share link | reads what the link shows |
| ask what would suit you | uses the taste profile you saved in ChefBear, if you allowed it (in the Claude app, when ChefBear offers it) |
| ask it to remember a taste or an allergy | adds it to your ChefBear taste profile (in the Claude app, when ChefBear offers it) |
| ask to share a menu, or to stop sharing | turns a public link on, after telling you anyone with it can see the dishes and prices, or off |

Claude is told to work only from the menu text and to remind you to confirm
allergens and ingredients with the restaurant.

The plugin helps even before you connect: Claude still reads menus in the
chat and helps you choose.

## Install

### Claude app and claude.ai

Install the plugin instead of the directory connector, so you don't end up
with two ChefBear connectors. Plugins need a paid Claude plan
([Use plugins in Claude](https://support.claude.com/en/articles/13837440))
and work on the web and in Claude Desktop. On the Free plan, add ChefBear
from the [Claude directory](https://claude.ai/directory/chefbear) instead.

1. Open **Customize → Plugins → Add → Add marketplace**, choose
   **Add from a repository** and enter `RaycarlLei/chefbear-skill`.
2. Install **ChefBear**, then connect it and approve in the ChefBear app.

### Claude Code

```
claude plugin marketplace add RaycarlLei/chefbear-skill
claude plugin install chefbear@chefbear
```

Then start a new Claude Code session, run `/mcp`, pick **chefbear**
(`plugin:chefbear:chefbear` when installed by the plugin), choose
**Authenticate**, and approve in the ChefBear app with the code shown on
ChefBear's sign-in page.

### Or paste one prompt

Copy the prompt in [INSTALL_PROMPT.md](../../INSTALL_PROMPT.md) into Claude.

## Before you connect

- A ChefBear account: get [ChefBear on the App Store](https://apps.apple.com/app/id6759192929)
  and sign in with Apple, Google or a verified email.
- ChefBear 4.4 or later on your iPhone, to approve the connection.
- Accounts in the mainland China service region can't connect ChefBear.

## What Claude can access

Only what you ask for, within the permissions you approve: your ChefBear
menus (save, import, list, read, and turn share links on or off) and, only
in the Claude app and if you allow it separately, your taste profile. It
can't place orders or take payments. Disconnect anytime in Claude, or in the
ChefBear app under **Settings › AI connections** (**Connected apps** in older
versions).

- Setup and help: https://www.chef-bear.com/claude/
- Privacy: https://www.chef-bear.com/privacy/ · Terms: https://www.chef-bear.com/terms/
- Support: https://www.chef-bear.com/support

MIT licensed.

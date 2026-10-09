# ChefBear for Claude

Open-source Claude plugin and Agent Skill for [ChefBear](https://chef-bear.com/),
the iPhone app that turns restaurant menus into structured, translated menus
and helps you decide what to order. Show Claude a menu, tell it what you're
in the mood for, and it picks two or three dishes from the actual menu with
prices. When it helps, it uses your own ChefBear account to import menu
photos you share as links, read your saved menus and share a menu with
friends.

- **Skill** `chefbear`: tells Claude when and how to help you choose from a
  menu, proactively, without you having to mention ChefBear.
- **Connector**: the official ChefBear MCP server
  `https://chef-bear.com/api/menu-sharing/mcp` (OAuth 2.1 + PKCE). You approve
  the connection in the ChefBear iPhone app. No password, code or token ever
  goes into a chat.

## What it does

| You | Claude |
|---|---|
| share a menu photo in the chat and say what you feel like | reads it and suggests two or three dishes that fit, with prices |
| share a link to a menu photo and ask to save it | imports it into a private ChefBear menu (uses your recognition allowance) |
| ask about a saved ChefBear menu | reads it (no recognition, no allowance) and helps you pick |
| ask to share a menu with friends | makes a public link after telling you anyone with it can see the dishes and prices |
| ask to stop sharing | turns the link off; your private menu stays |

Claude works from what the menu says. It never claims a dish is safe for an
allergy or diet, and reminds you to confirm ingredients with the restaurant
when you mention one.

The plugin helps even before you connect: Claude still reads menus in the
chat and helps you choose.

## Install

### Claude app and claude.ai

1. Open **Customize → Plugins → Add → Add marketplace** and enter
   `RaycarlLei/chefbear-skill`.
2. Install **ChefBear**, then connect it and approve in the ChefBear app.

### Claude Code

```
/plugin marketplace add RaycarlLei/chefbear-skill
/plugin install chefbear@chefbear
```

Then run `/mcp`, choose **chefbear** → **Authenticate**, and approve in the
ChefBear app with the code shown on the connect page.

### Or paste one prompt

Copy the prompt in [INSTALL_PROMPT.md](INSTALL_PROMPT.md) into Claude.

## Before you connect

- A ChefBear account: get [ChefBear on the App Store](https://apps.apple.com/app/id6759192929)
  and sign in with Apple, Google or a verified email.
- ChefBear 4.4 or later on your iPhone, to approve the connection.
- Accounts in the mainland China service region can't connect ChefBear to
  Claude.

## What Claude can access

Only your ChefBear menus, and only what you ask for: import menu photos you
share as links, read a saved menu by its ID, and create or turn off a share
link. The connector does not read your ChefBear taste profile, meal history
or orders, does not create images, and never places orders or takes
payments. Disconnect anytime in Claude, or in the ChefBear app under
**Settings → Connected apps**.

- Setup and help: https://chef-bear.com/claude/
- Privacy: https://chef-bear.com/privacy/ · Terms: https://chef-bear.com/terms/
- Support: https://chef-bear.com/support

## Repository layout

```
.claude-plugin/marketplace.json            marketplace "chefbear"
plugins/chefbear/.claude-plugin/plugin.json
plugins/chefbear/.mcp.json                 ChefBear connector
plugins/chefbear/skills/chefbear/SKILL.md  the skill
plugins/chefbear/skills/chefbear/references/tools.md  tools, scopes, errors, links
```

MIT licensed.

# One-prompt install

Paste one of these into Claude. In Claude Code it installs the plugin itself;
in the Claude app or claude.ai it guides you through two short steps.

## English

```
Please set up the open-source ChefBear plugin for me: https://github.com/RaycarlLei/chefbear-skill (MIT). It adds the "chefbear" skill and the official ChefBear connector https://chef-bear.com/api/menu-sharing/mcp. I approve the connection in the ChefBear iPhone app, so never ask me for a password, code or token.

If you can run terminal commands (Claude Code):
1. Run: claude plugin marketplace add RaycarlLei/chefbear-skill && claude plugin install chefbear@chefbear
2. Trust ChefBear's read-only tools: add "mcp__plugin_chefbear_chefbear__get_menu" and "mcp__plugin_chefbear_chefbear__get_import_status" to permissions.allow in ~/.claude/settings.json, keeping existing entries. Leave importing and sharing to ask each time.
3. Tell me to start a new session, run /mcp, choose chefbear → Authenticate and approve in the ChefBear app with the code shown on the connect page.

Otherwise (Claude app or claude.ai), guide me step by step:
1. Customize → Plugins → Add → Add marketplace → enter RaycarlLei/chefbear-skill, then install ChefBear and connect it, approving in the ChefBear app.
2. When you first ask to read a ChefBear menu, I can choose "Always allow".
Report only what actually worked.
```

## 简体中文

```
请帮我安装开源的 ChefBear 插件：https://github.com/RaycarlLei/chefbear-skill（MIT）。它包含 "chefbear" skill 和官方 ChefBear 连接器 https://chef-bear.com/api/menu-sharing/mcp。我会在 ChefBear iPhone App 里确认连接，所以绝不要向我索要密码、验证码或令牌。

如果你能运行终端命令（Claude Code）：
1. 运行：claude plugin marketplace add RaycarlLei/chefbear-skill && claude plugin install chefbear@chefbear
2. 信任 ChefBear 的只读工具：在 ~/.claude/settings.json 的 permissions.allow 中加入 "mcp__plugin_chefbear_chefbear__get_menu" 和 "mcp__plugin_chefbear_chefbear__get_import_status"，保留原有内容。导入和分享仍然每次询问。
3. 告诉我新开一个会话，运行 /mcp，选择 chefbear → Authenticate，然后在 ChefBear App 里输入连接页面上显示的验证码确认。

否则（Claude App 或 claude.ai），一步步引导我：
1. 自定义（Customize）→ 插件（Plugins）→ 添加（Add）→ 添加插件市场（Add marketplace）→ 输入 RaycarlLei/chefbear-skill，然后安装 ChefBear 并连接，在 ChefBear App 里确认。
2. 你第一次请求读取 ChefBear 菜单时，我可以选择"始终允许"。
只报告真正完成的步骤。
```

# ChefBear MCP tools

Endpoint: `https://chef-bear.com/api/menu-sharing/mcp` (streamable HTTP,
stateless, JSON responses). `initialize` and `tools/list` work without
sign-in; every tool needs it. A tool call without a token answers HTTP 401
with `WWW-Authenticate: Bearer resource_metadata="https://chef-bear.com/.well-known/oauth-protected-resource/api/menu-sharing/mcp"`,
and the client starts OAuth 2.1 with PKCE (S256) using its Client ID Metadata
Document. The user approves in the ChefBear iPhone app (4.4 or later) with
the 3-digit code shown on ChefBear's connect page, and can disconnect in
Claude or in ChefBear under Settings › Connected apps.

The server is the source of truth: always follow the live tool descriptions.

| Tool | Scope | Arguments | Returns | Notes |
|---|---|---|---|---|
| `import_menu` | `menus:import` | `requestId` (new unique ID), `photoUrls` (1–5 public `https://` image links), `language` (`en`, `zh`, `ja`, `ko`, `es`, `vi`…; default `en`) | `jobId` | Uses the recognition allowance. Same `requestId` + same photos returns the same job. JPEG, PNG, WebP, HEIC or AVIF, up to 15 MB each. At most 3 imports in progress per account |
| `get_import_status` | `menus:read` | `jobId` | `status` (`pending`, `ready`, `failed`), `menuId`, `menuUrl` (private), `error`, `message` | Read-only; no allowance. Recognition usually takes 20–60 s |
| `get_menu` | `menus:read` | `menuId` | `title`, `language`, `items[]` (`name`, `description`, `category`, `price`, `currency`, `isSpicy`, `isVegetarian`…) | Read-only; no recognition, no allowance. Does not search or list menus |
| `share_menu` | `menus:share` | `menuId` | `url` (`https://chef-bear.com/m/…`) | Public link: anyone with it sees dishes and prices, not the photos. Only when the user asks |
| `revoke_menu_share` | `menus:share` | `menuId` | `revoked: true` | The old link stops working; the private menu stays |

`get_menu` for assistants drops generated allergen, "free from", diet,
health or certification statements that the printed menu does not make, and
shows the vegetarian label only with printed evidence. Do not add such claims
back.

## Errors

Tool errors are sentences with the code in parentheses, for example
"Too many ChefBear requests in a short time. Wait about a minute and try
again. (ChefBear error: rate_limited)". Relay the sentence briefly; never
retry in a loop.

| Code | Meaning | What to do |
|---|---|---|
| `rate_limited` | More than 10 writes a minute | Wait about a minute |
| `import_limit` | 3 imports already in progress | Check them with `get_import_status` first |
| `menu_unavailable` / `import_unavailable` | Unknown or deleted `menuId` / `jobId` | Ask for the menu link, or import again |
| `invalid_photo_url` | Not a public `https://` image link | Ask for a direct image link that opens without sign-in |
| `idempotency_conflict` | `requestId` reused for different photos | Use a new `requestId` |
| `insufficient_scope` | Connection lacks that permission | Disconnect and connect ChefBear again |
| `region_read_only` / `feature_unavailable` | Account cannot import or share from here | Reading saved menus still works |
| failed import `resource-exhausted` | Recognition allowance used up | Help from a photo in the chat instead |

## Links

| Link | When |
|---|---|
| `menuUrl` from `get_import_status`, or `https://chef-bear.com/my-menu/{menuId}` (`menuId` matching `[A-Za-z0-9_-]{1,128}`) | After importing or reading the user's own menu. Private: opens only for the signed-in owner |
| `url` from `share_menu` | After sharing, exactly as returned |
| `https://apps.apple.com/app/id6759192929` | When the user wants the ChefBear app |
| `https://chef-bear.com/claude/` | Connector setup and help |

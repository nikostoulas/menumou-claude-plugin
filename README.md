# Menumou for Claude

Menumou lets restaurant owners edit their [Menumou](https://menumou.eu/en) menus from Claude:
list menus, search products, change prices, and add or remove products. The plugin connects
Claude to the Menumou MCP server at `https://menumou.eu/mcp`; there is no build step.

## Install

In Claude Code:

```
/plugin marketplace add nikostoulas/menumou-claude-plugin
/plugin install menumou@menumou
```

Then run `/mcp`, choose **menumou** and sign in with your Menumou account. Claude Code opens a
browser and receives the sign-in on `http://localhost:47823/callback`, so that port must be free.

In Claude (web, desktop, Cowork), install Menumou from the plugin directory and connect it from
the plugin's Connectors tab.

## Authentication

OAuth 2.1 with PKCE (`S256`) on the same origin. The public client is `menumou-claude`
(`token_endpoint_auth_method: none`) with scopes `menu:read` and `menu:write`.

- Authorize: `https://menumou.eu/oauth/authorize`
- Token: `https://menumou.eu/oauth/token`
- Revoke: `https://menumou.eu/oauth/revoke`
- Metadata: `https://menumou.eu/.well-known/oauth-protected-resource`

Menumou only accesses the menus owned by the connected account. Revoke access at any time from
`https://menumou.eu/<your-menu>/backend/plugin`; tool calls then return an "Authentication required"
error until you reconnect.

## Tools

| Tool                   | Scope        | Read-only | Destructive | What it does                                           |
| ---------------------- | ------------ | --------- | ----------- | ------------------------------------------------------ |
| `list_menus`           | `menu:read`  | yes       | no          | Lists the account's menus with their ids.              |
| `get_menu`             | `menu:read`  | yes       | no          | Returns a menu's categories, price columns and counts. |
| `search_products`      | `menu:read`  | yes       | no          | Searches products by name/description or category.     |
| `update_product_price` | `menu:write` | no        | yes         | Overwrites one price of one product.                   |
| `add_product`          | `menu:write` | no        | no          | Adds a product to a category, optionally with prices.  |
| `remove_product`       | `menu:write` | no        | yes         | Removes a product from the menu.                       |

Writes are visible on the public menu (`https://menumou.eu/<subdomain>`) right away.

## Example prompts

- "Show me my menus"
- "Set the Large Espresso price to €4.00"
- "Add Cheesecake to Desserts for €5.00"

## Support

- Website: https://menumou.eu/en
- Support: https://menumou.eu/en#contact
- Privacy policy: https://menumou.eu/privacy-policy?l=en
- Terms of service: https://menumou.eu/terms-of-service

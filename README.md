# Social0 MCP Server

[![npm](https://img.shields.io/npm/v/social0-mcp.svg)](https://www.npmjs.com/package/social0-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Social0 API](https://img.shields.io/badge/Social0-API-1a6b4a)](https://api.social0.app/docs)

Give your AI agent the ability to post to **9 social media platforms** from natural language.

**Supports:** Instagram, TikTok, YouTube, X (Twitter), LinkedIn, Facebook, Pinterest, Threads, Bluesky

Built on the [Social0 API](https://api.social0.app/docs). [Social0](https://social0.app) is a multi-platform social scheduler — write once, publish everywhere from one dashboard.

Requires **Node.js 20+** (used by `npx` under the hood).

## 1. Get an API key

1. Sign up at [social0.app](https://social0.app)
2. [Connect your social accounts](https://social0.app/dashboard/connections)
3. Create a key at [Dashboard → API keys](https://social0.app/dashboard/api-keys) (`sk_live_…`)

## 2. Add Social0 to your AI host

Pick your app. Paste the config, put in your API key, restart if needed.

### ChatGPT (Desktop)

ChatGPT supports MCP servers. Open:

**Settings → Connectors / MCP** (wording varies by version)

Add a server like:

```json
{
  "name": "social0",
  "command": "npx",
  "args": ["-y", "social0-mcp"],
  "env": {
    "SOCIAL0_API_KEY": "sk_live_your_key_here"
  }
}
```

Save and restart ChatGPT if needed. You should see the Social0 tools available.

> Availability varies by ChatGPT plan / region. Claude Desktop or Cursor are the most reliable today.

### Claude Desktop

Edit the MCP config file:

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "social0": {
      "command": "npx",
      "args": ["-y", "social0-mcp"],
      "env": {
        "SOCIAL0_API_KEY": "sk_live_your_key_here"
      }
    }
  }
}
```

Fully quit and reopen Claude Desktop.

### Cursor

**Settings → MCP** (or project `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "social0": {
      "command": "npx",
      "args": ["-y", "social0-mcp"],
      "env": {
        "SOCIAL0_API_KEY": "sk_live_your_key_here"
      }
    }
  }
}
```

Reload MCP / restart Cursor.

### VS Code (Copilot / MCP)

```json
{
  "servers": {
    "social0": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "social0-mcp"],
      "env": {
        "SOCIAL0_API_KEY": "sk_live_your_key_here"
      }
    }
  }
}
```

Exact file location depends on your VS Code MCP / Copilot settings UI.

### Same config for other hosts

Windsurf, Claude Code, and most stdio MCP hosts use the same shape:

```json
{
  "command": "npx",
  "args": ["-y", "social0-mcp"],
  "env": {
    "SOCIAL0_API_KEY": "sk_live_your_key_here"
  }
}
```

<details>
<summary>Optional: verbose logging</summary>

```json
"env": {
  "SOCIAL0_API_KEY": "sk_live_your_key_here",
  "SOCIAL0_MCP_VERBOSE": "true"
}
```

</details>

<details>
<summary>Developers: run from a git clone</summary>

Only needed if you are changing the server itself.

```bash
git clone https://github.com/Abhishek-B-R/social0-mcp.git
cd social0-mcp
npm install && npm run build
```

```json
{
  "mcpServers": {
    "social0": {
      "command": "node",
      "args": ["/absolute/path/to/social0-mcp/dist/index.js"],
      "env": {
        "SOCIAL0_API_KEY": "sk_live_your_key_here"
      }
    }
  }
}
```

</details>

## 3. Try it

Ask your AI:

- "Show my connected Social0 accounts"
- "Post this to LinkedIn and X"
- "Schedule a post for tomorrow at 9am UTC on Instagram and TikTok"
- "Upload this image from a URL and publish it everywhere"
- "Show my scheduled posts"
- "Which platforms should I use for this caption?"

## What your agent can do

- **Post** to any or all connected platforms in one step
- **Schedule** posts for any date/time
- **Upload** images and videos via public `url`, base64 `data`, or local `file_path`
- **Draft / edit / delete** posts before they go live
- **Track publish status** per platform (success, fail, or partial)
- **Per-platform options** — captions, TikTok / Instagram / YouTube / Pinterest settings, and more
- **Platform suggestions** — recommend where a caption fits best

Full tool reference: [AGENTS.md](./AGENTS.md)

## Supported platforms

Instagram · TikTok · YouTube · X (Twitter) · LinkedIn · Facebook · Pinterest · Threads · Bluesky

## Troubleshooting

### `SOCIAL0_API_KEY is required`

Put the key in the MCP config `env` block and reload MCP.

### Wrong key format

Keys start with `sk_live_` (legacy `s0_live_` still works). Create one at [social0.app/dashboard/api-keys](https://social0.app/dashboard/api-keys).

### `401 Unauthorized`

Key revoked or wrong — create a new key and update your config.

### `npx` / command not found

Use `"command": "npx"` with `"args": ["-y", "social0-mcp"]` as shown above. Requires Node.js 20+ on your PATH. First run may prompt to install the package — answer yes, or always pass `-y`.

### No connected account / multiple accounts

Connect platforms in the [dashboard](https://social0.app/dashboard/connections). If you have more than one account on a platform, tell the agent to use the account ID from “show my connected accounts.”

### Media upload failed / ENOENT

Remote hosts (Claude.ai, ChatGPT) cannot use sandbox file paths. Call `upload_media` with a public `url` or base64 `data` (+ `filename`). `file_path` only works for files on the machine running the MCP server.

### One platform failed, others succeeded

That’s normal — each platform publishes independently. Ask your agent to check publish status with the tracking ID.

## Links

- [npm: `social0-mcp`](https://www.npmjs.com/package/social0-mcp)
- [Social0](https://social0.app)
- [MCP docs](https://docs.social0.app/docs/integrations/mcp)
- [API reference](https://api.social0.app/docs)
- [Get an API key](https://social0.app/dashboard/api-keys)
- [Agent reference (AGENTS.md)](./AGENTS.md)

## License

MIT — see [LICENSE](LICENSE).

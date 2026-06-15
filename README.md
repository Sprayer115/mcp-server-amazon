# Amazon MCP Server

Forked from [rigwild/mcp-server-amazon](https://github.com/rigwild/mcp-server-amazon) with **multi-region / multi-locale support** — works with `amazon.com`, `amazon.de`, and other Amazon domains.

The server auto-detects your Amazon domain from the cookies you export, and adapts URL paths and locale-specific scraping strings accordingly.

## Features

- **Product search**: Search for products on Amazon
- **Product details**: Retrieve detailed information about a specific product on Amazon
- **Cart management**: Add items or clear your Amazon cart
- **Ordering**: Place orders (fake for demonstration purposes)
- **Orders history**: Retrieve your recent Amazon orders details

## Supported Regions

| Region | Domain | Status |
|--------|--------|--------|
| United States | `amazon.com` | ✅ Fully supported |
| Germany | `amazon.de` | ✅ Fully supported (locale-aware scraping) |
| Other | `amazon.co.uk`, `amazon.fr`, etc. | ⚠️ Should work — add locale strings in `src/config.ts` if needed |

## Demo

Simple demo, showcasing a quick product search and purchase.

![Demo GIF video](./demo.gif)

## Full Demo

Another more complex demo with products search, leveraging Claude AI recommendations to compare and make a decision, then purchase.

It showcases how natural and powerful the Amazon MCP integration could be inside a conversation

Video: https://www.youtube.com/watch?v=xas2CLkJDYg

## Install

Install dependencies

```sh
npm install -D
```

Build the project

```sh
npm run build
```

## Authentication

1. Log in to your Amazon account in a browser (e.g., Chrome or Firefox)
2. Install the [Cookie-Editor](https://chromewebstore.google.com/detail/cookie-editor/hlkenndednhfkekhgcdicdfddnkalmdm) browser extension
3. Export all cookies as JSON
4. Save them to `amazonCookies.json` in the project root

The server reads the domain from your cookies (e.g., `.amazon.de` or `.amazon.com`) and automatically adapts URL paths and scraping strings.

## Claude Desktop Integration

### macOS

Create or update `~/Library/Application Support/Claude/claude_desktop_config.json` with the path to the MCP server.

```json
{
  "mcpServers": {
    "amazon": {
      "command": "node",
      "args": ["/Users/admin/dev/mcp-server-amazon/build/index.js"]
    }
  }
}
```

### Windows

Create or update `%APPDATA%\Claude\claude_desktop_config.json` with the path to the MCP server.

```json
{
  "mcpServers": {
    "amazon": {
      "command": "node",
      "args": ["C:\\Users\\<YOUR_USER>\\dev\\mcp-server-amazon\\build\\index.js"]
    }
  }
}
```

Restart the Claude Desktop app to apply the changes. You should now see the Amazon MCP server listed in the Claude Desktop app.

## Troubleshooting

The MCP server logs its output to a file. If you encounter any issues, you can check the log file for more information.

- macOS: `~/Library/Logs/Claude/mcp-server-amazon.log`
- Windows: `%APPDATA%\Claude\mcp-server-amazon.log`

## License

[The MIT license](./LICENSE)

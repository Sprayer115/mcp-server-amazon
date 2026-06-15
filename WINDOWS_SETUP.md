# Windows Setup Guide — Amazon.de MCP Server

> Use this guide to set up the patched Amazon MCP Server on your Windows machine and configure it in Claude Desktop.

## Prerequisites

- Windows 10/11
- Node.js (v18+) — [download from nodejs.org](https://nodejs.org/)
- Git for Windows — [download from git-scm.com](https://git-scm.com/download/win)
- Chrome or Firefox (for cookie export)
- Claude Desktop installed

## Step 1 — Clone the repo

Open **PowerShell** or **Command Prompt** and run:

```powershell
cd C:\Users\%USERNAME%
git clone https://github.com/Sprayer115/mcp-server-amazon.git
cd mcp-server-amazon
```

## Step 2 — Install dependencies

```powershell
npm install -D
```

## Step 3 — Build the project

```powershell
npm run build
```

You should see a `build/` folder created with compiled `.js` files.

## Step 4 — Export your Amazon.de cookies

1. Open **Chrome** and go to [amazon.de](https://www.amazon.de)
2. Log in to your Amazon.de account
3. Install the **Cookie-Editor** extension:
   [Chrome Web Store → Cookie-Editor](https://chromewebstore.google.com/detail/cookie-editor/hlkenndednhfkekhgcdicdfddnkalmdm)
4. Click the Cookie-Editor icon in your toolbar
5. Click **Export** → **Export as JSON**
6. Copy the JSON output

## Step 5 — Save cookies to the project

In the `mcp-server-amazon` folder, create a file named `amazonCookies.json` and paste the exported JSON.

The file should look like this:

```json
[
  {
    "domain": ".amazon.de",
    "expirationDate": 1759999999,
    "hostOnly": false,
    "httpOnly": false,
    "name": "session-id",
    "path": "/",
    "sameSite": "Lax",
    "secure": true,
    "session": false,
    "storeId": "0",
    "value": "..."
  }
]
```

⚠️ **Important**: The `.amazon.de` domain in the cookie is what tells the server to use German locale strings and URLs.

## Step 6 — Configure Claude Desktop

1. Open or create the Claude Desktop config file:

```powershell
notepad "%APPDATA%\Claude\claude_desktop_config.json"
```

2. Paste this configuration (replace `<YOUR_USER>` with your actual Windows username):

```json
{
  "mcpServers": {
    "amazon": {
      "command": "node",
      "args": [
        "C:\\Users\\<YOUR_USER>\\mcp-server-amazon\\build\\index.js"
      ]
    }
  }
}
```

3. Save and close Notepad

## Step 7 — Restart Claude Desktop

Fully quit Claude Desktop (right-click tray icon → Quit) and reopen it.

## Step 8 — Verify it works

Open a new chat in Claude Desktop and ask:

> "Search Amazon for protein powder"

You should see the ✅ Amazon MCP server activate in the background and return results from **Amazon.de**.

## Troubleshooting

### "No cookies found" error
- Make sure `amazonCookies.json` is in the project root
- Verify the cookie `domain` field contains `.amazon.de`
- Re-export cookies while logged in to amazon.de

### Server not showing in Claude Desktop
- Check the config file path: `%APPDATA%\Claude\claude_desktop_config.json`
- Verify the `args` path points to the actual `build/index.js` file
- Check the log file: `%APPDATA%\Claude\mcp-server-amazon.log`

### Build errors
- Make sure you ran `npm install -D` before `npm run build`
- Ensure Node.js is v18 or newer: `node --version`

### German text not detected
- The server auto-detects locale from cookie domain
- If using `.com` cookies, it will use English strings
- For `.de`, it switches to German locale strings automatically

## Updating

When the repo is updated, pull the latest changes and rebuild:

```powershell
cd C:\Users\%USERNAME%\mcp-server-amazon
git pull origin main
npm run build
```

Then restart Claude Desktop.

## Adding more locales

If you want to add support for `amazon.co.uk`, `amazon.fr`, etc., edit `src/config.ts` and add entries to the `LOCALE_STRINGS` map:

```typescript
'amazon.co.uk': {
  emptyCart: 'Your Amazon Basket is empty',
  addedToCart: ['Added to Basket'],
  oneTimePurchase: 'One-time purchase',
  returnOrReplace: 'Return or replace items',
},
```

Then rebuild with `npm run build`.

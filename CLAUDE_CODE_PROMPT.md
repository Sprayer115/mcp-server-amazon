# Claude Code Prompt — Fix Windows Path & Rebuild

Run this in Claude Code on your Windows machine to pull the latest fix and rebuild:

```
In the mcp-server-amazon directory at C:\Users\<YOUR_USER>\mcp-server-amazon:
1. Run `git pull origin main` to get the latest fix (fileURLToPath for Windows compatibility)
2. Run `npm run build` to rebuild the TypeScript
3. Verify the build succeeded by checking that `build/index.js` exists and is recent
4. Report back the git log output (last 3 commits) and the build status
```

## What this fixes

The error you saw:
```
No amazonCookies.json file found at /C:/Users/iLux1/mcp-server-amazon/build//../amazonCookies.json
```

was caused by `new URL('.', import.meta.url).pathname` returning `/C:/Users/...` on Windows (invalid path with leading slash). The fix uses Node's `fileURLToPath` to resolve `file://` URLs to native Windows paths correctly.

## After this runs

1. Make sure `amazonCookies.json` exists in the project root (same folder as `package.json`)
2. Restart Claude Desktop
3. The MCP server should connect successfully

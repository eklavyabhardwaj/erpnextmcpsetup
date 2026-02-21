# ERPNext MCP Integration with Claude Desktop

Connect ERPNext to Claude Desktop using an MCP (Model Context Protocol)
server.

This setup allows Claude to securely interact with your ERPNext instance
via API --- fetching data, listing DocTypes, retrieving customers, and
more.

------------------------------------------------------------------------

## Architecture Overview

Claude Desktop\
↓\
MCP Server (Node.js)\
↓\
ERPNext REST API\
↓\
ERPNext Database

------------------------------------------------------------------------

# Requirements

-   Node.js (v18+ recommended)
-   Git
-   Claude Desktop
-   ERPNext instance with API access enabled

------------------------------------------------------------------------

# Step 1 --- Configure ERPNext API

1.  Login to ERPNext.
2.  Go to User List.
3.  Create a dedicated user (recommended: NOT Administrator).
4.  Assign appropriate roles.
5.  Click Generate API Key & Secret.
6.  Copy:
    -   API Key
    -   API Secret

### Test API Connectivity

Open in browser:

https://yourerp.com/api/method/ping

Expected response:

{"message":"pong"}

------------------------------------------------------------------------

# macOS Installation

## Clone MCP Server

cd \~/Projects mkdir Frappe_MCP cd Frappe_MCP git clone
https://github.com/rakeshgangwar/erpnext-mcp-server.git cd
erpnext-mcp-server npm install npm run build

Confirm build output:

build/index.js

------------------------------------------------------------------------

## Configure Claude Desktop (macOS)

Open:

\~/Library/Application Support/Claude/claude_desktop_config.json

Add:

{ "mcpServers": { "erpnext": { "command": "node", "args": \[
"/Users/YOUR_USERNAME/Projects/Frappe_MCP/erpnext-mcp-server/build/index.js"
\], "env": { "ERPNEXT_URL": "https://yourerp.com", "ERPNEXT_API_KEY":
"your_api_key", "ERPNEXT_API_SECRET": "your_api_secret" } } } }

Save and fully restart Claude.

------------------------------------------------------------------------

# Windows Installation

## Clone MCP Server

cd C:`\Users`{=tex}`\YourName`{=tex}`\Documents`{=tex} mkdir Frappe_MCP
cd Frappe_MCP git clone
https://github.com/rakeshgangwar/erpnext-mcp-server.git cd
erpnext-mcp-server npm install npm run build

Confirm:

build`\index`{=tex}.js

------------------------------------------------------------------------

## Configure Claude Desktop (Windows)

Open:

C:`\Users`{=tex}`\YourName`{=tex}`\AppData`{=tex}`\Roaming`{=tex}`\Claude`{=tex}`\claude`{=tex}\_desktop_config.json

Add:

{ "mcpServers": { "erpnext": { "command": "node", "args": \[
"C:\\Users\\YourName\\Documents\\Frappe_MCP\\erpnext-mcp-server\\build\\index.js"
\], "env": { "ERPNEXT_URL": "https://yourerp.com", "ERPNEXT_API_KEY":
"your_api_key", "ERPNEXT_API_SECRET": "your_api_secret" } } } }

Restart Claude completely.

------------------------------------------------------------------------

# Testing the Integration

After restarting Claude Desktop, test with:

List available ERPNext tools.

Then try:

Get 5 customers.

If working correctly: - Claude will show a tool call - Data will be
returned from ERPNext

------------------------------------------------------------------------

# Manual Debugging

## Validate JSON Config

cat claude_desktop_config.json \| python -m json.tool

## Test MCP Server Directly

node build/index.js

## Test ERPNext API

curl https://yourerp.com/api/method/ping

------------------------------------------------------------------------

# Security Best Practices

-   Do NOT use Administrator API keys
-   Regenerate keys if exposed
-   Use HTTPS only
-   Create a dedicated "AI User"
-   Assign minimum required roles

------------------------------------------------------------------------

# Project Structure

Frappe_MCP/ └── erpnext-mcp-server/ ├── build/ │ └── index.js ├──
package.json └── README.md

------------------------------------------------------------------------

# Final Checklist

-   ERPNext API reachable
-   Node installed
-   MCP server built
-   JSON config valid
-   Claude restarted
-   Tool calls visible

------------------------------------------------------------------------

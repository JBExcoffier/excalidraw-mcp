# Excalidraw MCP App Server

MCP server that streams hand-drawn Excalidraw diagrams with smooth viewport camera control and interactive fullscreen editing.

![Demo](docs/demo.gif)

## Install

Works with any client that supports [MCP Apps](https://modelcontextprotocol.io/docs/extensions/apps) — Claude, ChatGPT, VS Code, Goose, and others. If something doesn't work, please [open an issue](https://github.com/antonpk1/excalidraw-mcp-app/issues).


### Local

**Build from Source**

```bash
git clone https://github.com/excalidraw/excalidraw-mcp.git
cd excalidraw-mcp-app
pnpm install && pnpm run build
```

#### _Cursor_

To use this MCP from **Cursor** so the AI can draw Excalidraw sketches in chat:

1. **Build the server** (if you haven’t already):
   ```bash
   cd /path/to/excalidraw-mcp
   npm install
   npm run build
   ```
   This produces `dist/index.js`.

2. **Configure the MCP in Cursor** using one of these:

   **Option A — Project config (recommended)**  
   A `.cursor/mcp.json` is already in this repo. It points at this project’s `dist/index.js`. After building, just restart Cursor and the Excalidraw server will be available when this folder is the workspace.

   **Option B — Global config**  
   Create or edit `~/.cursor/mcp.json` and add:
   ```json
   {
     "mcpServers": {
       "excalidraw": {
         "command": "node",
         "args": ["/absolute/path/to/excalidraw-mcp/dist/index.js", "--stdio"]
       }
     }
   }
   ```
   Replace `/absolute/path/to/excalidraw-mcp` with the real path (e.g. `~/Desktop/JBE/TESTS/excalidraw-mcp` or a full path).

   **Option C — Cursor UI**  
   - Open **Cursor Settings** (Cmd + Shift + J on Mac, Ctrl + Shift + J on Windows/Linux).  
   - Go to **Tools & MCP** → **Add new MCP server**.  
   - Set **Name** to `excalidraw`.  
   - Set **Type** to `command`.  
   - **Command**: `node`.  
   - **Args**: `["/absolute/path/to/excalidraw-mcp/dist/index.js", "--stdio"]` (use your actual path).

3. **Restart Cursor** so it picks up the MCP.

4. **Use it in chat**  
   In a Cursor chat, ask to draw something (e.g. “Draw a simple flowchart with three boxes and arrows” or “Sketch a system architecture with a client, API, and database”). The AI will use the Excalidraw MCP tools (`read_me` and `create_view`) to produce a diagram in the chat. You can approve tool use when prompted or enable auto-run in **Tools & MCP** if you prefer.

**Troubleshooting**

- **MCP not listed**: Confirm `dist/index.js` exists after `npm run build`, then restart Cursor fully.  
- **Logs**: **View** → **Output** (Cmd + Shift + U / Ctrl + Shift + U) → choose **MCP Logs**.  
- **Cloud Agents**: Use the project-level `.cursor/mcp.json` so cloud agents can see the server.

## Usage

Example prompts:
- "Draw a cute cat using excalidraw"
- "Draw an architecture diagram showing a user connecting to an API server which talks to a database"

## What are MCP Apps and how can I build one?

Text responses can only go so far. Sometimes users need to interact with data, not just read about it. [MCP Apps](https://github.com/modelcontextprotocol/ext-apps/) is an official Model Context Protocol extension that lets servers return interactive HTML interfaces (data visualizations, forms, dashboards) that render directly in the chat.

- **Getting started for humans**: [documentation](https://modelcontextprotocol.io/docs/extensions/apps)
- **Getting started for AIs**: [skill](https://github.com/modelcontextprotocol/ext-apps/blob/main/plugins/mcp-apps/skills/create-mcp-app/SKILL.md)

## Contributing

PRs welcome! See [Local](#local) above for build instructions.

### Deploy your own instance

You can deploy your own copy to Vercel in a few clicks:

1. Fork this repo
2. Go to [vercel.com/new](https://vercel.com/new) and import your fork
3. No environment variables needed — just deploy
4. Your server will be at `https://your-project.vercel.app/mcp`

### Release checklist

<details>
<summary>For maintainers</summary>

```bash
# 1. Bump version in manifest.json and package.json
# 2. Build and pack
pnpm run build && mcpb pack .

# 3. Create GitHub release
gh release create v0.3.0 excalidraw-mcp-app.mcpb --title "v0.3.0" --notes "What changed"

# 4. Deploy to Vercel
vercel --prod
```

</details>

## Credits

Built with [Excalidraw](https://github.com/excalidraw/excalidraw) — a virtual whiteboard for sketching hand-drawn like diagrams.

## License

MIT

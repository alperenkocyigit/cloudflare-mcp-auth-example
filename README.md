# Remote MCP Server Example on Cloudflare (With Token Authentication)

This example allows you to deploy a remote MCP server that requires token-based authentication on Cloudflare Workers.

## Get started: 

[![Deploy to Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/cloudflare/ai/tree/main/demos/remote-mcp-authless)

This will deploy your MCP server to a URL like: `remote-mcp-server-auth.<your-account>.workers.dev/sse`

Alternatively, you can use the command line below to get the remote MCP Server created on your local machine:
```bash
npm create cloudflare@latest -- my-mcp-server --template=cloudflare/ai/demos/remote-mcp-authless
```

## Token Authentication

To secure your MCP server, token-based authentication is implemented. Only requests with a valid secret token are authorized.

### Setting up the secret token

1. Define your secret token in the Cloudflare Worker environment variables.
   - Add `SECRET_TOKEN` to your `wrangler.toml` file or set it directly in the Cloudflare dashboard.

2. Example `wrangler.toml` configuration:
   ```toml
   [vars]
   SECRET_TOKEN = "your-secret-token"
   ```

### Using the token

Include the `Authorization` header with the token in your requests:
```bash
curl -H "Authorization: Bearer your-secret-token" https://remote-mcp-server-auth.<your-account>.workers.dev/sse
```

## Customizing your MCP Server

To add your own [tools](https://developers.cloudflare.com/agents/model-context-protocol/tools/) to the MCP server, define each tool inside the `init()` method of `src/index.ts` using `this.server.tool(...)`. 

## Connect to Cloudflare AI Playground

You can connect to your MCP server from the Cloudflare AI Playground, which is a remote MCP client:

1. Go to https://playground.ai.cloudflare.com/
2. Enter your deployed MCP server URL (`remote-mcp-server-auth.<your-account>.workers.dev/sse`)
3. You can now use your MCP tools directly from the playground!

## Connect Claude Desktop to your MCP server

You can also connect to your remote MCP server from local MCP clients, by using the [mcp-remote proxy](https://www.npmjs.com/package/mcp-remote). 

To connect to your MCP server from Claude Desktop, follow [Anthropic's Quickstart](https://modelcontextprotocol.io/quickstart/user) and within Claude Desktop go to Settings > Developer > Edit Config.

Update with this configuration:

```json
{
  "mcpServers": {
    "calculator": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "http://localhost:8787/sse"  // or remote-mcp-server-auth.your-account.workers.dev/sse
      ]
    }
  }
}
```

Restart Claude and you should see the tools become available.

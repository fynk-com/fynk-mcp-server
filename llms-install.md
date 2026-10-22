# Installing the fynk MCP server

This is a **remote** MCP server. Do not clone this repository, install dependencies, or run a local process. There is no package to install.

## Steps

1. Add the server to the client's MCP configuration:

   ```json
   {
     "mcpServers": {
       "fynk": {
         "type": "http",
         "url": "https://app.fynk.com/mcp"
       }
     }
   }
   ```

2. Do not ask the user for an API key, token, client ID or client secret. This server uses OAuth 2.1 with dynamic client registration, and none of these exist.
3. Connect. The server responds with `401` and a `WWW-Authenticate` header that points to its protected-resource metadata. Follow the standard MCP OAuth flow.
4. Give the user the authorization URL to open in a browser. They sign in to fynk and approve the connection.
5. Once approved, list the tools to verify. A user with full permissions sees 68 tools; users with fewer permissions see fewer. If the list is empty, authentication did not complete.

## Tell the user about these if authentication fails

- They need a fynk account on the Essential, Business or Pro plan, or an active 14-day trial. The Free plan does not include MCP access.
- An account owner must turn MCP on under Account settings → MCP. Other users can request access from their profile.

## Do not

- Configure a local transport, a command, or environment variables.
- Change the URL or the transport. `url` must be exactly `https://app.fynk.com/mcp`. `type` is the transport name, `http` (Streamable HTTP), not the URL scheme; clients that infer the transport from the URL can leave it out.

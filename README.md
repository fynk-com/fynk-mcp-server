# fynk MCP server

> Find, analyze, draft and sign contracts in fynk from Claude, ChatGPT, Cursor or any AI client.

This repository documents the hosted MCP server for [fynk](https://fynk.com/), a European contract lifecycle management platform. The server itself is hosted by fynk. There is nothing to install, build or run locally, and this repository contains no server code.

**68 tools: 20 read-only, 48 that make changes.** Most contract platforms with an MCP server stop at reading. This one runs the whole lifecycle: draft a new template, create contracts from it or from an uploaded PDF, fill dynamic fields, add parties and signatories, set metadata, move it through review and signing, tag it, hand it to a team and archive it. Your assistant only sees the tools your own fynk permissions allow.

## Connection details

| | |
| --- | --- |
| Endpoint | `https://app.fynk.com/mcp` |
| Transport | Streamable HTTP (remote) |
| Auth | OAuth 2.1 with PKCE and dynamic client registration. No API keys, no client secrets |
| Requirements | A fynk account on Essential, Business or Pro, or an active 14-day trial. An account owner turns MCP on under Account settings → MCP |
| MCP Registry | [`com.fynk.app/mcp`](https://registry.modelcontextprotocol.io/v0/servers?search=com.fynk.app/mcp) |

> This repository holds documentation and configuration only. The server is operated by fynk GmbH and is not open source.

## Install

Every client ends the same way: you sign in to fynk in your browser and approve the connection.

**Claude**: open [fynk in the Claude directory](https://claude.ai/directory/fynk) and click **Connect**.

**ChatGPT** (Pro or Business): open [chatgpt.com/plugins](https://chatgpt.com/plugins) and click **+** next to the search box. If there is no **+**, turn on Settings → Security and login → **Developer mode** first. Name the plugin `fynk`, paste the endpoint into **Connection**, tick **I understand and want to continue**, click **Create**, then **Sign in with fynk**.

**Cursor**: [Add to Cursor](https://cursor.com/en/install-mcp?name=fynk&config=eyJ1cmwiOiJodHRwczovL2FwcC5meW5rLmNvbS9tY3AifQ%3D%3D), or Settings → MCP → **Add new MCP server** with name `fynk` and the endpoint above.

**VS Code**: [Add to VS Code](https://vscode.dev/redirect/mcp/install?name=fynk&config=%7B%22name%22%3A%22fynk%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapp.fynk.com%2Fmcp%22%7D), then approve the install.

**Claude Code**

```bash
claude mcp add --transport http fynk https://app.fynk.com/mcp
```

Then run `/mcp` in a session, select fynk and choose **Authenticate**.

**Codex**

```bash
codex mcp add fynk --url https://app.fynk.com/mcp
```

`codex mcp login fynk` runs the sign-in again if access is ever revoked.

**Microsoft Copilot Studio**: in your agent, click **+** next to Tools, choose **Model Context Protocol** and click **Add**. Name `fynk`, description `fynk MCP Server`, the endpoint above, authentication **OAuth 2.0**, configuration type **Dynamic (with Discovery)**. Make sure the agent uses the signed-in user's permissions, not admin-level access.

**Any client configured by file**, such as Cline or Zed (also in [`.mcp.json`](.mcp.json)):

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

## What your assistant can do

- **Draft**: write a complete template from scratch, such as an NDA or a service agreement, with its parties, dynamic fields and signature blocks, then create contracts from it with the fields already filled in
- **Find and analyze**: search your contracts, open one, read its summary, fields and metadata, download the latest PDF, and answer questions about it
- **Create**: from a template, from scratch, by uploading a PDF, or by duplicating an existing contract
- **Fill in**: dynamic fields, metadata values, tags and owning teams
- **Parties and signatories**: add, update and remove them
- **Move it along**: review, signing and done, with a clear list of anything that blocks the next stage
- **Retire**: archive and restore
- **Templates**: read, duplicate, rewrite and publish them, and manage their dynamic fields, parties and signature blocks
- **Account**: read users, teams, tags, metadata, document types and playbooks, and manage tags, teams, metadata and document types if you have the permission

Example prompts: [fynk MCP collection](https://help.fynk.com/en/articles/760207-fynk-mcp-collection)

### Creating contracts from PDFs

This takes two steps: the assistant asks fynk for a signed upload URL, then sends the PDF to that URL itself with an HTTPS POST. The upload happens outside MCP, so it only works where the assistant can send files over the network:

- Coding agents such as Claude Code, Cursor, VS Code and Codex can run the upload as a command on your machine; you may be asked to approve it.
- In Claude, turn on Settings → Capabilities → **Allow network access** first.
- Where the assistant can't make network requests, upload the PDF in fynk directly and continue from there.

## Security

- **It acts as you.** The assistant inherits exactly the permissions of the user who connected it and never reaches further.
- **Nothing to rotate.** OAuth handles credentials. Access tokens expire after an hour, connections after three months without use.
- **Admins hold the switch.** MCP is off until an account owner turns it on, and every connection is visible and revocable in one place.
- **Every action on record.** Everything the assistant does lands in the document audit trail.
- **EU infrastructure.** Hosted on AWS within the EEA, ISO 27001 certified, GDPR compliant by design, eIDAS signatures (SES, AES, QES).

**One honest caveat:** once your contract data reaches a connected third-party AI client, fynk cannot guarantee it stays in the EU. If you need strict EU data residency, use the built-in fynk AI assistant instead of MCP. See the [MCP privacy notice](https://fynk.com/en/legal/terms/mcp-privacy-notice/).

## What it deliberately cannot do

- AI review, AI draft and AI search stay inside fynk and are not exposed as tools.
- Playbooks can be listed but not run.
- Approval checkpoints can be read but not approved. The assistant names the approver; a person still decides.
- It works from the PDF and a structured summary rather than raw clause text, so broad portfolio questions work well and forensic single-clause questions do not.

## Links

- [MCP server overview](https://fynk.com/en/integrations/mcp-server/)
- [Setup guide](https://help.fynk.com/en/articles/759041-connect-fynk-to-your-ai-assistant-with-mcp)
- [Announcement post](https://fynk.com/en/blog/fynk-mcp-server/)
- [MCP privacy notice](https://fynk.com/en/legal/terms/mcp-privacy-notice/)
- [Contact](https://fynk.com/en/contact/)

## License

MIT, see [LICENSE](LICENSE). The license covers only the documentation and configuration files in this repository. It does not cover the fynk service, which is proprietary and operated by fynk GmbH, or the fynk name and logo; see [NOTICE](NOTICE).

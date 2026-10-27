# Melonhelp MCP servers

Remote [Model Context Protocol](https://modelcontextprotocol.io) servers for the [Melonhelp](https://melonhelp.com) suite: help desk, AI support agents, tasks and product feedback. Connect them to Claude, ChatGPT, Cursor or any MCP client and work with your support data from the chat or from your coding agent.

All four servers are **remote (Streamable HTTP) with OAuth 2.1**. There is nothing to install, no API key and no local process: paste the URL, sign in with your Melonhelp account in the browser and pick the workspace.

| Product | Server URL | Access | Plans |
|---|---|---|---|
| [Melonhelp Tickets](https://melonhelp.com/tickets) | `https://tickets.melonhelp.com/mcp` | Read only | All plans. An admin turns it on first |
| [Melonhelp Agents](https://melonhelp.com/agents) | `https://agents.melonhelp.com/mcp` | Read and write | All plans, Free included. The workspace owner turns it on first |
| [Melonhelp Tasks](https://melonhelp.com/tasks) | `https://tasks.melonhelp.com/mcp` | Read and write | All plans, Free included |
| | `https://tasks.melonhelp.com/mcp/readonly` | Read only | |
| [Melonhelp Feedback](https://melonhelp.com/feedback) | `https://feedback.melonhelp.com/mcp` | Read and write | All plans, Free included |
| | `https://feedback.melonhelp.com/mcp/readonly` | Read only | |

## Connect

**Claude (web or desktop):** Settings, Connectors, Add custom connector. Paste the server URL and click Connect. A browser window opens to sign in and choose the workspace. On Team and Enterprise plans an owner adds the connector for the organization first.

**ChatGPT:** add it as a custom MCP connector (developer mode) and authorize in the browser. Workspace admins may need to approve it first.

**Cursor, Windsurf, VS Code and other clients:** add a remote server with the URL. Clients that support OAuth discovery open the sign-in page on their own.

Every server publishes its OAuth metadata at `/.well-known/oauth-protected-resource` and `/.well-known/oauth-authorization-server`, supports dynamic client registration and requires PKCE (S256). Connections are scoped to the workspace you pick and can be revoked from the product's settings.

## Tools

### Melonhelp Tickets

Help desk for WhatsApp, email and Slack. Read only (`tickets:read`): ask about your queue, a customer's history or your metrics without leaving the chat.

`list_tickets`, `get_ticket`, `get_ticket_comments`, `get_metrics`, `list_filters`

Turn it on in Settings, Webhooks and MCP. Turning it off cuts live connections.

### Melonhelp Agents

AI agents that answer customers on your website, WhatsApp and inside Melonhelp Tickets. Build and tune them from the chat.

- **Knowledge:** `generate_faqs`, `add_faqs`, `add_website`, `add_text`
- **Skills:** `list_skills`, `get_skill`, `create_skill`, `update_skill`
- **Agents:** `list_agents`, `get_agent`, `clone_agent`, `update_agent`, `set_agent_skills`
- **Actions (HTTP tools):** `list_tools`, `create_http_tool`, `update_tool`, `delete_tool`, `set_agent_tools`
- **Flows:** `get_flow_format`, `list_flows`, `get_flow`, `create_flow`, `update_flow`, `simulate_flow`, `publish_flow`, `delete_flow`, `set_agent_flows`
- **Usage and diagnostics:** `get_usage`, `get_conversation_usage`, `list_conversations`, `get_conversation`

Tools respect the plan limits: on Free you get one agent and five knowledge sources, and actions start on the Entry plan.

### Melonhelp Tasks

Projects and tasks for teams, with time tracking. Create and update work from your AI client or coding agent.

`list_workspaces`, `list_projects`, `list_statuses`, `list_members`, `search_tasks`, `get_task`, `list_my_tasks`, `create_task`, `update_task`, `add_comment`, `log_time`

`log_time` appears on plans with time tracking. The `/mcp/readonly` URL exposes only the read tools.

### Melonhelp Feedback

Feedback button, public portal with votes and roadmap, changelog, surveys and error capture. Your coding agent installs the SDK with your real key and works through what users report.

- **Read:** `check_installation`, `get_install_guide`, `get_issue`, `get_response`, `get_summary`, `list_errors`, `list_issues`, `list_portals`, `list_widgets`, `search_responses`
- **Write:** `add_allowed_origins`, `add_note`, `claim_issue`, `configure_launcher`, `configure_theme`, `configure_widget`, `create_survey`, `draft_changelog`, `link_pull_request`, `register_release`, `release_issue`, `reply_to_user`, `resolve_issue`
- **Prompts:** `install_feedback`, `add_error_reporting`, `add_survey`

Write tools need a workspace admin and a connection that allows writing. Members with a read-only role use `/mcp/readonly`.

## Example prompts

- "Which WhatsApp tickets have been waiting more than 4 hours?" (Tickets)
- "Add our shipping policy page to the support agent's knowledge and test it with three questions." (Agents)
- "Create a task in the Website project for the checkout bug and assign it to me." (Tasks)
- "Install Melonhelp Feedback in this Next.js app with error capture, then check the installation." (Feedback)

## Security

- OAuth 2.1 with PKCE. No long-lived API keys to leak.
- Each connection is limited to the workspace you choose on the consent screen.
- Read-only URLs (Tickets, and `/mcp/readonly` on Tasks and Feedback) expose no write tools at all.
- Admins can turn each server off and revoke connections from the product's settings.
- Feedback returns text written by end users marked as untrusted, so agents treat it as data, not instructions.

## Related

- [Melonhelp/skills](https://github.com/Melonhelp/skills): agent skills that use these servers
- [Melonhelp/feedback-js](https://github.com/Melonhelp/feedback-js): JavaScript SDK for Melonhelp Feedback
- Docs: [melonhelp.com/docs](https://melonhelp.com/docs)

## License

The contents of this repository are MIT licensed. The servers are hosted services operated by Melonhelp.

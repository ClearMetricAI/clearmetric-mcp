# ClearMetric MCP

Connect your AI to ClearMetric to build and run business apps, query shared
records, measure business results and propose changes under your existing access
and review rules. ClearMetric hosts the service. This repository contains its
public documentation and connection metadata.

[ClearMetric](https://www.clearmetric.ai) · [Request access](https://www.clearmetric.ai)
· [Public catalog](catalog.json) · [Release verification](release.json)

## Connect

You need an active ClearMetric account and access to a business. Request access
on the website if you do not have one. Your AI client's subscription and workspace
permissions are separate requirements.

If your account requires authenticator verification, complete it in
[ClearMetric Me](https://app.clearmetric.ai/me) before authorizing your AI client.

The remote server uses Streamable HTTP with OAuth sign-in.

```text
https://api.clearmetric.ai/mcp
```

Sign in, choose the business and select Allow. The connection stays bound to that
business. Switching businesses in the console does not move the AI connection.
Disconnect and authorize again to select another business. Owners can manage
connected AI clients in ClearMetric's access controls.

### Claude

1. In Claude web or Desktop, open Customize → Connectors → Add → Add custom connector. Name it ClearMetric and paste this server address.
2. Choose OAuth sign-in and Register automatically. Connect, sign in to ClearMetric, choose your business and select Allow.
3. Start a conversation and enable ClearMetric from the + menu under Connectors. Paste the starter prompt.

A Team or Enterprise workspace may need its owner to add the connector first.

[Official instructions](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).
Documentation checked 2026-11-21. Client connection tested not recorded.

### ChatGPT

1. Open Plugins → + → Add custom MCP server. Name it ClearMetric, paste this server address and choose OAuth authentication.
2. Review ChatGPT’s connection notice and choose Create as a plugin. Sign in to ClearMetric, choose your business and select Allow. Review the discovered tools.
3. Install the plugin, start a conversation and type @ to select ClearMetric. Paste the starter prompt and confirm actions when prompted.

Custom MCP access depends on your plan and workspace permissions. If your account shows the earlier Apps interface, use Settings → Apps → Create, choose OAuth, Scan Tools and Create.

[Official instructions](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt).
Documentation checked 2026-11-21. Client connection tested not recorded.

### Cursor

1. Add the configuration below to .cursor/mcp.json in your project. Keep any other servers already configured.
2. Open Cursor’s MCP settings and connect ClearMetric. Complete the browser sign-in, choose your business and select Allow.
3. Open Agent with ClearMetric enabled and paste the starter prompt.

Cursor configuration

```
{
  "mcpServers": {
    "clearmetric": {
      "url": "https://api.clearmetric.ai/mcp"
    }
  }
}
```

[Official instructions](https://cursor.com/docs/mcp).
Documentation checked 2026-11-21. Client connection tested not recorded.

### Claude Code

1. Run the command below in your project to add ClearMetric as an HTTP MCP server.
2. In Claude Code, run /mcp, choose ClearMetric and authenticate. Sign in in the browser, choose your business and select Allow.
3. Return to Claude Code and paste the starter prompt.

Claude Code command

```
claude mcp add --transport http clearmetric https://api.clearmetric.ai/mcp
```

[Official instructions](https://code.claude.com/docs/en/mcp).
Documentation checked not recorded. Client connection tested not recorded.

### Codex

1. Add the configuration below to your Codex configuration at ~/.codex/config.toml. Keep any other servers already configured.
2. Run codex mcp login clearmetric, or authenticate ClearMetric in MCP settings. Sign in in the browser, choose your business and select Allow.
3. Open a new Codex conversation with ClearMetric enabled and paste the starter prompt.

Codex configuration

```
[mcp_servers.clearmetric]
url = "https://api.clearmetric.ai/mcp"
```

[Official instructions](https://developers.openai.com/codex/mcp).
Documentation checked not recorded. Client connection tested not recorded.

### Other MCP client

1. Add a remote MCP server named ClearMetric using the server address above. Choose HTTP or Streamable HTTP transport.
2. Use OAuth with automatic client registration. Sign in to ClearMetric, choose your business and select Allow.
3. Enable ClearMetric’s tools in your conversation and paste the starter prompt.

Your client must support remote HTTP MCP servers and OAuth sign-in.

[Official instructions](https://modelcontextprotocol.io/docs/develop/connect-remote-servers).
Documentation checked not recorded. Client connection tested not recorded.

## Start a conversation

Ask your AI to use ClearMetric to confirm the connected business, describe what
you can access and explain the next permitted step. For example, ask it to help
plan an app for your team's process, find records you can read or explain a
business metric. It should call `describe_model` before naming business fields,
metrics or actions. Use names returned for your business rather than assuming
sample entities exist.

```text
Use ClearMetric to confirm my connected business and whether it is a test or
production business. Read the model I am permitted to see, then help me plan
an app for my team's process. Ask for missing details and show me the plan.
```

## Tools

The service defines 18 tools. Discovery and execution follow your
permissions. The [public catalog](catalog.json) includes their complete runtime
descriptions. These tools work with the business definitions you are permitted
to see.

| Tool | Purpose | Access |
| --- | --- | --- |
| `submit_feedback` | Send feedback to ClearMetric | Business permissions |
| `describe_model` | Describe the model | Business permissions |
| `search` | Search records | Business permissions |
| `query` | Query an entity | Business permissions |
| `get_record` | Get one record | Business permissions |
| `measure` | Measure a metric | Business permissions |
| `act` | Run an action | Business permissions |
| `list_apps` | List apps | Business permissions |
| `propose_change` | Propose a change | Business permissions |
| `propose_app` | Propose an app | Business permissions |
| `list_proposals` | List proposals | Business permissions |
| `get_proposal` | Get a proposal | Business permissions |
| `propose_access` | Propose an access change | owner |
| `invite` | Invite a person | owner |
| `view_as` | Preview a person's permissions | owner |
| `access_summary` | Access summary | owner |
| `connect_source` | Connect a source | owner |
| `sync_status` | Sync status | manager, owner |

## Connectors

These implemented source types come from the shared connector registry. Actual
availability depends on ClearMetric configuration, provider registration, your
plan and your permissions. Owners can use `connect_source` without a connector
to inspect current setup availability in their business.

| Connector | Purpose | Authentication |
| --- | --- | --- |
| GitHub (`github_oauth`) | Read application source from the GitHub repositories you select | `oauth` |
| GitHub repository (`github`) | Read application source from one authorized GitHub repository | `secret` |
| CSV file (`csv`) | A CSV file uploaded by a person | `none` |
| Google Sheets (shared with ClearMetric) (`google_sheets`) | A Google Sheets file shared with ClearMetric's account Use `google` for new connections when this path is unavailable. | `service_account` |
| Google Sheets (`google`) | The sheets you pick from your Google account | `oauth` |
| A database of ours (`sql_database`) | Your own Postgres or MySQL database, read only | `secret` |
| An API (`rest_api`) | Any other tool with an API | `secret` |
| QuickBooks Online (`quickbooks`) | Invoices, bills, payments, customers and hours | `oauth` |
| HubSpot (`hubspot`) | Companies, contacts, deals and tickets | `secret` |
| Stripe (`stripe`) | Customers, invoices, payments and payouts | `secret` |
| Salesforce (`salesforce`) | Accounts, contacts, leads and opportunities | `oauth` |
| Shopify (`shopify`) | Orders, customers and products | `client_credentials` |
| Microsoft 365 (`microsoft`) | Excel tables and SharePoint lists | `oauth` |
| Google Drive (`google_drive`) | Google Drive: the files in a folder shared with ClearMetric's account Requires ClearMetric platform configuration. | `service_account` |

Source connections import or read external records and application source as
described above. They do not imply arbitrary write access to an external service.
Credentials are entered through ClearMetric's connection flow. OAuth provider
consent and ClearMetric business-change approval are separate steps.

## Access and approvals

Your signed-in identity and selected business determine access. Record, field,
app and action permissions apply to AI operations through the same engine used
by the console. A tool's availability does not grant access to every record.

<details>
<summary>Runtime instructions supplied to AI clients</summary>

ClearMetric Engine. Call describe_model first to learn the entities, fields, metrics, actions and apps this person may name; every other tool takes those names. You see exactly what the signed in person sees. Every write uses the shared pipeline. The returned status and needs say whether it applied or requires review or confirmation. Do not request another approval or code for an applied change. Pending changes are not done; approval controls stay in the console and there is no approve tool. Identity, business and environment come from the authenticated context, never extra tool arguments. Resolve business uncertainty through the shared resolution field, not invented meanings.
Read describe_model and its permitted meanings first. Reuse supplied facts and current definitions.
An ordinary clear read needs no interview or saved plan. If materially different interpretations remain,
ask one concrete question. On query, measure, search, get_record or act, resolution op ask can save two to
eight complete candidate argument sets for that same tool. Choices are client proposals, not verified
business facts. Keep labels concise and use only permitted definitions and records. Present the returned
server question and choice IDs. Submit resolution op answer with its id, revision and selected choice ID,
then follow its actual result. Never claim an MCP answer proves human approval. It changes no shared
meaning or permission. Shared definitions use the existing requirements and model proposal path.
Use op get to resume or inspect a decision and op cancel to cancel pending work. Never repeat a write
after an uncertain response; inspect its original proposal/outcome. A changed model or access needs fresh
context. Missing data, missing permissions and unsupported capabilities are different from an unanswered
business choice. Never fabricate zero or disguise an unsupported workflow as ordinary records.


</details>

The AI client may also ask you to authorize a tool call. That client confirmation
does not replace ClearMetric's business review. Disconnecting or revoking a
client removes its ClearMetric connection authority. Each AI provider processes
data under its own terms, which you should review before connecting it.

## Troubleshooting

If custom MCP is missing, check the client's current plan and workspace policy
using its official guide. If sign-in fails, confirm you have business access and
reconnect through OAuth. If a tool is denied, ask your business owner to review
your permissions. If a change is pending, open its returned ClearMetric review
link. A request for review does not mean the change has applied.

If the server returns `mfa_required`, verify your account with an authenticator
in ClearMetric, then disconnect and authorize the AI client again. Email sign-in
codes do not satisfy this account-security requirement. Reconnect after account
security changes if the server says your account verification ended.

## Version and maintenance

The server reports version 3.1.1. [Registry metadata](server.json)
provides a remote service definition and does not establish a published Registry
listing. Each directory has its own submission and acceptance process.

These files are generated from ClearMetric's shared runtime definitions and
client setup catalog. [Release verification](release.json) records the deployed
engine revision and published file hashes. Client documentation review dates
are separate from successful authenticated connection tests. A metadata check
alone does not establish a working client connection.

For support or access requests, contact ClearMetric through the website. Report
issues without credentials, tokens or private business records.

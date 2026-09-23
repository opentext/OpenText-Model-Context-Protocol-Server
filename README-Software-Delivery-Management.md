# OpenText — Model Context Protocol Server

*Software Delivery Management*

[![OpenText Software Delivery Management](https://img.shields.io/badge/OpenText-ALM_Octane-00A9E0?logoColor=white)](https://www.opentext.com/products/software-delivery-management) [![Model Context Protocol compatible](https://img.shields.io/badge/Model_Context_Protocol-compatible-000000?logo=modelcontextprotocol&logoColor=white)](https://modelcontextprotocol.io) [![Documentation: administrator guide](https://img.shields.io/badge/Documentation-Administrator_Guide-00A9E0)](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-overview.htm) [![Authentication: OAuth 2.1 or access key](https://img.shields.io/badge/Authentication-OAuth_2.1%20%7C%20Access_Key-2EBC4F)](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-overview.htm) [![Hosting: on-premise and cloud](https://img.shields.io/badge/Hosting-On--premise_%26_Cloud-00A9E0)](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-overview.htm)

[Overview](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-overview.htm) · [Administrator setup](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-admin-setup.htm) · [Connect a client](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-connecting-ai-client.htm) · [Custom integrations](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-advanced-setup.htm) · [Troubleshooting](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-troubleshooting.htm)

> Looking for OpenText SaaS instead? See [README-Core-Software-Delivery-Platform.md](README-Core-Software-Delivery-Platform.md). For the shared overview, see [README.md](README.md).

---

The official [Model Context Protocol](https://modelcontextprotocol.io) server for **Software Delivery Management** — embedded directly inside the product, with no local proxy or additional process required. Once enabled, your AI tools get real-time read and write access to your backlog, work items, and delivery data. Authentication uses **OAuth 2.1** or **API access keys**, so every action respects the user's existing workspace permissions.

> **Availability — version 26.3:** The Model Context Protocol feature is in gradual exposure mode and is currently disabled by default. Contact OpenText Support to enable it for your site or tenant.

---

## At a glance

| | |
| --- | --- |
| **Product** | Software Delivery Management |
| **Protocol** | Stateless HTTP — POST for tool calls, GET for endpoint discovery; JSON-RPC 2.0 |
| **Authentication** | OAuth 2.1 (interactive browser flow) · Access key (headless or service use) |
| **Hosting** | Embedded in Software Delivery Management |
| **Release** | Version 26.3, gradual exposure |
| **Documentation base** | `https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/` |

---

## Contents

- [At a glance](#at-a-glance)
- [What you can do](#what-you-can-do)
- [How it works](#how-it-works)
- [Supported tools](#supported-tools)
- [Before you start](#before-you-start)
- [Set up authentication](#set-up-authentication)
- [Connect your AI client](#connect-your-ai-client)
- [More example prompts](#more-example-prompts)
- [Tips and tricks](#tips-and-tricks)
- [Security](#security)
- [Support and feedback](#support-and-feedback)

---

## What you can do

Connect your AI client to your workspace and interact with delivery data without leaving your IDE:

| Capability | Example prompt |
| --- | --- |
| Search and filter | *"List all open defects assigned to me in the current sprint."* |
| Find by keyword | *"Find all stories in the backlog that mention performance."* |
| Create work items | *"Create five user stories from these meeting notes."* |
| Update entities | *"Update the description of story #1042 with these acceptance criteria."* |
| Explore metadata | *"What fields does a Feature entity have?"* |
| Check status | *"What is the current status of epic #205?"* |

---

## How it works

The server runs embedded and is reachable over HTTPS at the `/mcp` path.

- **Transport:** Stateless HTTP — POST for tool calls, GET for endpoint discovery
- **Protocol:** JSON-RPC 2.0; every request is self-contained
- **Authentication:** OAuth 2.1 (browser login flow) or access key (static bearer token)
- **Permissions:** Every action is scoped to the authenticated user's workspace access

### Endpoint

| Deployment | Base address |
| --- | --- |
| On-premise or private cloud | `https://<your-software-delivery-management-host>/mcp` |

---

## Supported tools

The server exposes the following tools:

| Tool | Description |
| --- | --- |
| `get_entity_types` | Lists entity types available in the workspace |
| `get_filter_metadata` | Returns filter syntax and supported operators |
| `get_entity_field_metadata` | Shows fields and field types for an entity type |
| `get_entities` | Lists entities with optional filtering, search, sorting, and pagination |
| `get_entity` | Retrieves a single entity by identifier |
| `create_entity` | Creates a new entity |
| `update_entity` | Updates selected fields on an existing entity |

For full tool specifications and schema details, see [Build custom integrations](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-advanced-setup.htm).

---

## Before you start

### Prerequisites

- An active Software Delivery Management license
- The Model Context Protocol feature enabled by your OpenText administrator (contact Support if it is not yet enabled)
- One of the supported AI clients (see [Connect your AI client](#connect-your-ai-client))
- For OAuth 2.1: a modern browser to complete the sign-in flow
- For an access key: an access token generated in the target workspace
- An Aviator AI license for each agent or user that connects
- Each connection consumes a named user license, the same as any regular user accessing the workspace

### Administrator prerequisites

Before users can connect, a site administrator must:

1. Enable the Model Context Protocol feature (contact OpenText Support)
2. Choose and configure authentication — see [Set up authentication](#set-up-authentication)
3. For OAuth 2.1: register each client in the `OAUTH_AUTHORIZATION_SERVER_CLIENTS` site parameter

Full administrator instructions: [Enable the server and register clients](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-admin-setup.htm)

---

## Set up authentication

Two authentication methods are supported. Choose based on your use case.

| Method | Best for |
| --- | --- |
| **OAuth 2.1** | Interactive IDE clients; no long-lived credentials in configuration files |
| **Access key** | Headless or service setups; static token in the client configuration |

### OAuth 2.1

Submit a support ticket to enable OAuth 2.1. Once enabled, set the following site parameters:

| Parameter | Value |
| --- | --- |
| `OAUTH_AUTHORIZATION_SERVER_ENABLED` | `true` |
| `OAUTH_AUTHORIZATION_SERVER_CLIENTS` | JSON array of registered client definitions |

**Redirect addresses by client:**

| Client | Redirect addresses to register |
| --- | --- |
| Claude Desktop | `http://127.0.0.1/oauth/callback`, `http://localhost/oauth/callback` |
| Visual Studio Code with GitHub Copilot | `http://127.0.0.1:33418/`, `https://vscode.dev/redirect` |
| IntelliJ with GitHub Copilot | `http://127.0.0.1:33428/callback`, `http://127.0.0.1/callback` |
| Cursor | `http://127.0.0.1/oauth/callback`, `http://localhost/oauth/callback` |

**Example client registration:**

```json
[
  {
    "client_id": "copilot_vscode",
    "client_name": "GitHub Copilot (Visual Studio Code)",
    "redirect_uris": [
      "http://127.0.0.1:33418/",
      "https://vscode.dev/redirect"
    ]
  },
  {
    "client_id": "claude_desktop",
    "client_name": "Claude Desktop",
    "redirect_uris": [
      "http://127.0.0.1/oauth/callback",
      "http://localhost/oauth/callback"
    ]
  }
]
```

Changes to `OAUTH_AUTHORIZATION_SERVER_CLIENTS` take effect immediately — no server restart is needed.

OAuth 2.1 metadata is available at: `<server>/.well-known/oauth-authorization-server`

### Access key

Create an access bearer token in the target workspace (use the **Token** type). For details, see [API access](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/how_setup_APIaccess.htm).

> **Caution:** Avoid embedding access tokens as plain text in configuration files. Where the client supports it, use an environment variable or a client-managed input variable instead.

---

## Connect your AI client

Find the configuration snippet for your client below. Replace `https://myoctane.company.com/mcp` with your actual server address.

### Visual Studio Code with GitHub Copilot

Open the generated `mcp.json` file in Visual Studio Code and add one of the following entries:

**OAuth 2.1:**
```json
{
  "servers": {
    "opentext-mcp": {
      "type": "http",
      "url": "https://myoctane.company.com/mcp"
    }
  }
}
```

**Access key:**
```json
{
  "servers": {
    "opentext-mcp": {
      "type": "http",
      "url": "https://myoctane.company.com/mcp",
      "headers": {
        "Authorization": "Bearer ${input:octaneAccessToken}"
      }
    }
  }
}
```

Visual Studio Code prompts once for the token value on first use and stores it in its secure secret storage.

After saving, open Copilot Chat, select **Tools**, and verify that the tools are listed.

### IntelliJ with GitHub Copilot

Open the `mcp.json` file in IntelliJ and add the same entries shown for Visual Studio Code above.

### Claude Desktop

**Configuration file location:**
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

**OAuth 2.1:**
```json
{
  "mcpServers": {
    "opentext-mcp": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://myoctane.company.com/mcp",
        "--static-oauth-client-info",
        "{\"client_id\": \"<client_id>\"}"
      ]
    }
  }
}
```

**Access key:**
```json
{
  "mcpServers": {
    "opentext-mcp": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://myoctane.company.com/mcp",
        "--header",
        "Authorization: Bearer <your-access-token>"
      ]
    }
  }
}
```

Requires Node.js version 18 or later for the `mcp-remote` proxy.

### Cursor

Create or edit `.cursor/mcp.json` at the project root:

**OAuth 2.1:**
```json
{
  "mcpServers": {
    "opentext-mcp": {
      "url": "https://myoctane.company.com/mcp"
    }
  }
}
```

**Access key:**
```json
{
  "mcpServers": {
    "opentext-mcp": {
      "url": "https://myoctane.company.com/mcp",
      "headers": {
        "Authorization": "Bearer <your-access-token>"
      }
    }
  }
}
```

### Verify the connection

After configuration:

- **Visual Studio Code and IntelliJ:** Open Copilot Chat, select **Tools**, and confirm that the ALM Octane tools are listed and enabled
- **Claude Desktop:** Open the tools picker in a chat and confirm that the tools appear
- **Cursor:** Open Composer and confirm that the tools are available in the agent tools list

If the tools are not visible, recheck the configuration file path and the server address, then restart the client. See [Troubleshooting](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-troubleshooting.htm).

---

## More example prompts

A few additional things you can ask your AI client:

- *"What entity types are available in my workspace?"*
- *"Create a defect titled 'Login fails on Safari 17' with high severity."*
- *"Find all stories in the backlog that mention performance."*

---

## Tips and tricks

### Set a default workspace in your agent instructions

Add the following to your `AGENTS.md` file (or the equivalent instructions file) to skip discovery calls on every session and reduce token usage:

```md
## OpenText Model Context Protocol Server

When connected to the opentext-mcp server:
- **MUST** use workspace identifier = <your-workspace-id>
- **MUST** use shared space identifier = <your-shared-space-id>
- **MUST** use base address = "https://myoctane.company.com"
```

### Use natural language for filtering

The `get_entities` tool supports rich filtering. Describe what you need in natural language and let the client translate it to the correct filter syntax by calling `get_filter_metadata` first.

---

## Security

All traffic is encrypted in transit over HTTPS using TLS 1.2 or later. Access is strictly bounded by the authenticated user's existing workspace permissions in ALM Octane; the server cannot grant broader access than the user already has.

Use of the server is subject to the OpenText End User License Agreement, available on the [OpenText agreements page](https://www.opentext.com/agreements), and to the applicable service description for your deployment.

---

## Support and feedback

- **Administrator guide:** [Model Context Protocol server overview](https://admhelp.microfocus.com/octane/en/26.3/Online/Content/AdminGuide/mcp-server-overview.htm)
- **Support tickets:** [OpenText Support Portal](https://support.opentext.com/)
- **Community:** Submit feature requests through [OpenText Community Ideas](https://community.opentext.com/devops-cloud/software-delivery-mgmt/i/ideas)

---

## Disclaimer

The server performs actions in OpenText ALM Octane with the permissions of the authenticated user. Apply least privilege, review high-impact changes before confirming, and monitor audit logs for unusual activity.

# OpenText Model Context Protocol Server

[![OpenText](https://img.shields.io/badge/OpenText-ValueEdge_%26_ALM_Octane-00A9E0?logoColor=white)](https://www.opentext.com/products/software-delivery-management) [![Model Context Protocol compatible](https://img.shields.io/badge/Model_Context_Protocol-compatible-000000?logo=modelcontextprotocol&logoColor=white)](https://modelcontextprotocol.io) [![Authentication: OAuth 2.1 or access key](https://img.shields.io/badge/Authentication-OAuth_2.1%20%7C%20Access_Key-2EBC4F)](https://modelcontextprotocol.io) [![Hosting: on-premise and cloud](https://img.shields.io/badge/Hosting-On--premise_%26_Cloud-00A9E0)](https://www.opentext.com)

The official [Model Context Protocol](https://modelcontextprotocol.io) server for OpenText delivery products — embedded directly inside the product, with no local proxy or additional process required. Once enabled, your AI tools get real-time read and write access to your backlog, work items, and delivery data. Authentication uses OAuth 2.1 or API access keys, so every action respects the user's existing workspace permissions.

---

## Choose your guide

This repository documents two products. They share the same protocol, the same tool set, and the same client configuration, but they are built on different documentation bases and release lines. Pick the guide that matches your deployment.

| Your deployment | Also known as | Guide |
| --- | --- | --- |
| **Core Software Delivery Platform** | OpenText ValueEdge | **[README-Core-Software-Delivery-Platform.md](README-Core-Software-Delivery-Platform.md)** |
| **Software Delivery Management** | OpenText ALM Octane | **[README-Software-Delivery-Management.md](README-Software-Delivery-Management.md)** |


---

## Contents

- [Choose your guide](#choose-your-guide)
- [What the server does](#what-the-server-does)
- [What you can do](#what-you-can-do)
- [Supported tools](#supported-tools)
- [Shared prerequisites](#shared-prerequisites)
- [Shared setup outline](#shared-setup-outline)
- [Security](#security)
- [Support and feedback](#support-and-feedback)

---

## What the server does

The Model Context Protocol server runs embedded inside the product and is reachable over HTTPS at the `/mcp` path.

| | |
| --- | --- |
| **Protocol** | Stateless HTTP — POST for tool calls, GET for endpoint discovery; JSON-RPC 2.0 |
| **Authentication** | OAuth 2.1 (interactive browser flow) · API access key (headless or service use) |
| **Hosting** | Embedded in the product — on-premise and cloud |
| **Permissions** | Every action is scoped to the authenticated user's workspace access |

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

## Supported tools

Both products expose the same tool set:

| Tool | Description |
| --- | --- |
| `get_entity_types` | Lists entity types available in the workspace |
| `get_filter_metadata` | Returns filter syntax and supported operators |
| `get_entity_field_metadata` | Shows fields and field types for an entity type |
| `get_entities` | Lists entities with optional filtering, search, sorting, and pagination |
| `get_entity` | Retrieves a single entity by identifier |
| `create_entity` | Creates a new entity |
| `update_entity` | Updates selected fields on an existing entity |

---

## Shared prerequisites

These apply equally to both products:

- An active product site, either on-premise or cloud
- The Model Context Protocol feature enabled by your OpenText administrator
- One of the supported AI clients: Visual Studio Code with GitHub Copilot, IntelliJ with GitHub Copilot, Claude Desktop, or Cursor
- For OAuth 2.1: a modern browser to complete the sign-in flow
- For an access key: an API access token generated in the target workspace
- An Aviator AI license for each agent or user that connects
- Each connection consumes a named user license, the same as any regular user accessing the workspace

---

## Shared setup outline

The detailed, link-accurate instructions live in the product guides. At a high level, the sequence is identical for both products:

1. **Enable the feature.** A site administrator submits a support ticket to have the Model Context Protocol enabled for the site or tenant.
2. **Choose an authentication method.** OAuth 2.1 for interactive clients, or an access key for headless and service setups.
3. **Register your clients** (OAuth 2.1 only) in the `OAUTH_AUTHORIZATION_SERVER_CLIENTS` site parameter, including the redirect addresses for each client.
4. **Configure your AI client** with the server address and the chosen credentials.
5. **Verify the connection** by listing the available tools in your client.

Continue in **[README-Core-Software-Delivery-Platform.md](README-Core-Software-Delivery-Platform.md)** for OpenText ValueEdge, or **[README-Software-Delivery-Management.md](README-Software-Delivery-Management.md)** for OpenText ALM Octane.

---

## Security

All traffic is encrypted in transit over HTTPS using TLS 1.2 or later. Access is strictly bounded by the authenticated user's existing workspace permissions; the server cannot grant broader access than the user already has.

> **Caution:** Avoid embedding access tokens as plain text in configuration files. Where the client supports it, use an environment variable or a client-managed input variable instead.

Use of the server is subject to the OpenText End User License Agreement, available on the [OpenText agreements page](https://www.opentext.com/agreements), and to the applicable service description for your deployment.

---

## Support and feedback

- **Support tickets:** [OpenText Support Portal](https://support.opentext.com/)
- **Community:** Submit feature requests through [OpenText Community Ideas](https://community.opentext.com/devops-cloud/software-delivery-mgmt/i/ideas)

---

## Disclaimer

The server performs actions in the product with the permissions of the authenticated user. Apply least privilege, review high-impact changes before confirming, and monitor audit logs for unusual activity.

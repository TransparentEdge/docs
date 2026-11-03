---
description: >-
  Query your Transparent Edge account in natural language from Claude, Codex,
  Antigravity and other AI assistants through the Model Context Protocol (MCP).
---

# AI assistants (MCP)

The **Transparent Edge MCP server** connects AI assistants such as Claude, Codex and Antigravity to your Transparent Edge account. Once connected, you can ask about your traffic, cache performance, security events and configuration in plain language, and the assistant fetches the answer from your account:

* _"What was the hit ratio of www.example.com over the last hour?"_
* _"Which IPs triggered the most WAF rules today, and from which countries?"_
* _"Show me the top URLs returning 5xx errors in the last 4 hours."_
* _"Is there any DDoS activity on my sites this morning?"_
* _"How much traffic did we serve last month?"_

It is based on the open [Model Context Protocol](https://modelcontextprotocol.io), so it works with any MCP-compatible client.

{% hint style="info" %}
The connector is **read-only**: it can query your account, but it never changes configuration, purges content or modifies anything.
{% endhint %}

### Server URL

```
https://mcp.transparentedge.eu/mcp
```

### How you sign in

Claude, Codex and Antigravity sign you in with your usual Transparent Edge account through your browser, including two-factor authentication if your account uses it. You never have to paste API keys, and the assistant only gets access once you authorize it.

Each assistant uses its own **OAuth client ID**, listed in its section below. These IDs are public and the same for every customer: they identify the assistant, not you.

### Connecting from Claude

Works in Claude on the web, desktop and mobile. You add the connector once and it follows your account.

1. In Claude, go to **Customize → Connectors** and click **Add custom connector**.
2. Name it **Transparent Edge** and paste the server URL: `https://mcp.transparentedge.eu/mcp`.
3. Open **Advanced settings** and paste this **OAuth Client ID**. Leave the client secret empty.

   ```
   0eCt5kaQensfmXHRideDYGAPw3DOVhPI8nFDKWtR
   ```
4. Click **Add**, then **Connect**, and sign in with your Transparent Edge account.

That's it. Ask Claude anything about your account, and it will use the connector when it needs to.

{% hint style="info" %}
On the Free plan Claude allows one custom connector. On Team and Enterprise plans, an owner adds the connector in **Organization settings → Connectors**, and each member then clicks **Connect** and signs in with their own Transparent Edge user.
{% endhint %}

To disconnect, go back to **Customize → Connectors** and remove Transparent Edge.

### Connecting from Codex

The Codex app, the CLI and the IDE extension share one configuration, so you only set this up once.

1. Add the server from a terminal:

   ```bash
   codex mcp add transparent-edge --url https://mcp.transparentedge.eu/mcp --oauth-client-id pTcdb5IZlEPyb7mQK3yTOBEKLRJ0ZnqwjXmMeH42
   ```
2. Sign in. Codex opens your browser on the Transparent Edge login page:

   ```bash
   codex mcp login transparent-edge
   ```
3. Log in with your Transparent Edge account. When the browser confirms it, go back to Codex and restart it if it was already open.

The first command writes this to `~/.codex/config.toml`, so you can also add it by hand and then run `codex mcp login transparent-edge`:

```toml
[mcp_servers.transparent-edge]
url = "https://mcp.transparentedge.eu/mcp"

[mcp_servers.transparent-edge.oauth]
client_id = "pTcdb5IZlEPyb7mQK3yTOBEKLRJ0ZnqwjXmMeH42"
```

### Connecting from Antigravity

1. In the agent panel, open the **⋯** menu and go to **MCP Servers → Manage MCP Servers → View raw config**. That opens `~/.gemini/config/mcp_config.json`.
2. Add this block and save:

   ```json
   {
     "mcpServers": {
       "transparent-edge": {
         "serverUrl": "https://mcp.transparentedge.eu/mcp",
         "oauth": {
           "clientId": "3pXw8uj58eM15s2oaEjqBi7EmtNvfft5AcvH51uO"
         }
       }
     }
   }
   ```
3. Back in **Manage MCP Servers**, refresh. Antigravity opens your browser on the Transparent Edge login page. Sign in, and the server shows up with its tools.

### Connecting with API credentials

Clients that can't sign in through a browser (for example a machine without one, or MCP clients without OAuth support) can authenticate with your API credentials instead, sent as HTTP headers on every request:

| Header               | Value              |
| -------------------- | ------------------ |
| `X-TE-Client-Id`     | Your client ID     |
| `X-TE-Client-Secret` | Your client secret |

You'll find both values in your [dashboard user profile](https://dashboard.transparentcdn.com/user/profile), the same ones used for the [API](../guides/api/authentication.md).

This is how most clients' configuration file looks, using the `mcp-remote` bridge (requires Node.js 18 or later):

```json
{
  "mcpServers": {
    "transparent-edge": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote", "https://mcp.transparentedge.eu/mcp",
        "--transport", "http-only",
        "--header", "X-TE-Client-Id:${TE_CLIENT_ID}",
        "--header", "X-TE-Client-Secret:${TE_CLIENT_SECRET}"
      ],
      "env": {
        "TE_CLIENT_ID": "YOUR_CLIENT_ID",
        "TE_CLIENT_SECRET": "YOUR_CLIENT_SECRET"
      }
    }
  }
}
```

In Codex, add `env_http_headers = { "X-TE-Client-Id" = "TE_CLIENT_ID", "X-TE-Client-Secret" = "TE_CLIENT_SECRET" }` to the server's block in `~/.codex/config.toml` and export both variables. In Antigravity, add a `"headers"` object with both headers next to `serverUrl`.

{% hint style="warning" %}
Your client secret gives full access to the API on your behalf. Keep the configuration file private and never commit it to a repository.
{% endhint %}

### Working with several companies

If your user has access to more than one company, the connector starts with the first one. Ask the assistant to list your companies or to switch to another one, for example _"switch to the company Acme"_, and every following query uses that company. You can only reach the companies your own user has access to.

### What the assistant can query

| Area | What you can ask about |
| --- | --- |
| **Account** | Your user, the companies you can access, and the services each one has contracted. |
| **Sites and configuration** | Sites, backends (origins), active VCL configuration, SSL certificates, network ACLs and Transparent Edge's IP ranges. |
| **CDN analytics** | Traffic over time and breakdowns by site, URL, status code, country, region, client IP, ASN, user agent, referer, JA3, protocol, cache status and response time. |
| **Backend analytics** | Origin latency (TTFB, connect, fetch) over time and breakdowns by origin, path, status code and more. |
| **WAF** | Security events over time, triggered rules, top attacking IPs, targeted paths and countries, and each site's WAF configuration. |
| **Anti-DDoS** | Attacks and mitigations over time, top IPs, ASNs, countries, URLs, user agents and protocols. |
| **Bot Management** | Detected bots, actions taken, crawlers, detection reasons, and distribution by country and URL. |
| **Security report** | A combined overview of CDN, WAF, Anti-DDoS and bot activity in a single request. |
| **IP and JA3 reputation** | Geolocation, ASN, reputation score and threats for an IP, plus the user agents, IPs and sites associated with an IP or a JA3 fingerprint. |
| **Historic traffic and consumption** | Long-term traffic and billable consumption per service. |
| **Operations** | Purge history, configured alerts and the users of your company. |

WAF, Anti-DDoS and Bot Management data is available only for companies that have those services contracted.

### Limits

* **Time range of real-time analytics.** Queries over detailed analytics can cover up to **4 hours**, or up to **1 month** with the Dedicated Analytics service. For longer periods the assistant uses historic data, which has no range limit.
* **Time zones.** The assistant sends dates with an explicit time zone offset. If you care about a specific one, mention it (_"yesterday from 9:00 to 13:00 Madrid time"_).
* **Heavy queries.** Some queries, such as consumption over long periods, are computed in the background. If the result isn't ready within a few seconds, the assistant receives a reference and can pick up the result with a follow-up request, without starting over.

### Security and privacy

* The connector acts with **your own user's permissions**. It can't see anything you couldn't see from the dashboard or the API.
* The connector doesn't store the data it retrieves. Each session lives in memory only and is discarded after 12 hours of inactivity.
* The data the connector returns is sent to your AI assistant's provider and is subject to that provider's privacy policy.

See our [privacy policy](https://www.transparentedge.eu/en/privacy-policy/) for details.

### Troubleshooting

<details>

<summary>The assistant asks me to sign in again</summary>

Your authorization has expired or was revoked. Sign in again: **Connect** in Claude, `codex mcp login transparent-edge` in Codex, or refresh in Antigravity's **Manage MCP Servers**.

</details>

<details>

<summary>"Service not contracted"</summary>

The active company doesn't have the service you asked about (WAF, Anti-DDoS or Bot Management). Check that the right company is active, or contact your account manager.

</details>

<details>

<summary>"Range exceeded"</summary>

The requested period is longer than your real-time analytics allow (4 hours, or 1 month with Dedicated Analytics). Ask for a shorter period or for historic data.

</details>

<details>

<summary>The assistant says I'm not authenticated (API credentials)</summary>

Check that both headers, `X-TE-Client-Id` and `X-TE-Client-Secret`, are set and match the values in your dashboard profile.

</details>

If the problem persists, contact [support](mailto:help+cdn@transparentedge.eu).

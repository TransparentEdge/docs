---
description: >-
  Query your Transparent Edge account in natural language from Claude and
  other AI assistants through the Model Context Protocol (MCP).
---

# AI assistants (MCP)

The **Transparent Edge MCP server** connects AI assistants such as Claude to your Transparent Edge account. Once connected, you can ask about your traffic, cache performance, security events and configuration in plain language, and the assistant fetches the answer from your account:

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

### Connecting from Claude

Claude (web, desktop and mobile) connects with your usual Transparent Edge login. You never have to paste API keys.

1. In Claude, go to **Customize → Connectors** and search for **Transparent Edge**.
2. Click **Connect**.
3. Sign in with your Transparent Edge username and password. If your account uses two-factor authentication, you'll be asked for your code as usual.
4. Review the permissions and click **Authorize**.

That's it. Ask Claude anything about your account, and it will use the connector when it needs to.

{% hint style="info" %}
You can also connect first and sign in later: the first time Claude needs your data, it shows a **Connect** button right in the conversation.
{% endhint %}

To disconnect, go back to **Customize → Connectors** and remove Transparent Edge.

### Connecting from other MCP clients

Clients that don't support OAuth sign-in (for example Cursor, VS Code or command-line tools) can authenticate with your API credentials instead, sent as HTTP headers on every request:

| Header               | Value              |
| -------------------- | ------------------ |
| `X-TE-Client-Id`     | Your client ID     |
| `X-TE-Client-Secret` | Your client secret |

You'll find both values in your [dashboard user profile](https://dashboard.transparentcdn.com/user/profile), the same ones used for the [API](../guides/api/authentication.md).

This is how most clients' configuration file looks, using the `mcp-remote` bridge:

```json
{
  "mcpServers": {
    "transparent-edge": {
      "command": "npx",
      "args": [
        "-y", "mcp-remote",
        "https://mcp.transparentedge.eu/mcp",
        "--header", "X-TE-Client-Id: YOUR_CLIENT_ID",
        "--header", "X-TE-Client-Secret: YOUR_CLIENT_SECRET"
      ]
    }
  }
}
```

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

<summary>Claude asks me to connect again</summary>

Your authorization has expired or was revoked. Click **Connect** and sign in again.

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

<summary>The assistant says I'm not authenticated (other MCP clients)</summary>

Check that both headers, `X-TE-Client-Id` and `X-TE-Client-Secret`, are set and match the values in your dashboard profile.

</details>

If the problem persists, contact [support](mailto:support@transparentedge.eu).

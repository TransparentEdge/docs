# Sites Overview

Sites Overview gives you a single-page health check for any of your sites — traffic, security, and configuration status, all in one place, without having to jump between different sections of the dashboard. You'll find it under **Insights → Sites Overview**.

This is a new addition to the panel — there's no direct equivalent in the previous dashboard, though it pulls together information that used to live scattered across Analytics, WAF, Bot Management, and configuration screens.

### How to use it

Pick a site from the dropdown at the top of the page. The rest of the page updates to show that site's current status, split into four blocks:

#### Health

* **Certificate** — whether the site has a valid SSL/TLS certificate, or is being served over HTTP only. If you don't have one yet, you'll see a direct link to request one.
* **Tunnel** — whether the site has a tunnel bound to it.
* **Backends serving it** — whether backend traffic has been recorded for this site in the last 24 hours, with a link to the full backend analytics.

\{% hint style="info" %\} Some of these indicators may appear differently depending on your plan — for example, if a feature like Tunnels isn't included in your contract, you may see an option to enable it rather than its current status. \{% endhint %\}

#### Traffic · Last 24h

Four key numbers, each with a mini trend graph: **Requests**, **Bandwidth**, **Cache hit** (%), and **Error rate** (%). Click **Open in Analytics** to jump straight into the full Delivery report for this site, filtered and ready to go.

#### Security

* **WAF positives** — how many requests were flagged by the firewall in the last 24 hours.
* **Bot Management** — whether bot detection is enabled for this site.
* **Under Attack** — whether Under Attack mode is currently active for this site.
* **Anomaly detection** — how many tiers (Delivery, WAF, Bot Mitigation...) are currently being monitored for anomalies on this site.

Each card links directly to where you'd go to change that setting (e.g. "WAF analytics", "Configure bots", "Under Attack mode", "Set up monitoring").

#### Configuration

* **Easy Setup rules** — how many rules currently target this site, or a note that it's using company-wide defaults.
* **API Protection** — whether an API schema is being enforced for this site.
* **Safe Routes** — how many safe routes are defined.
* **DNS zone** — whether a delegated DNS zone covers this site.

As with Security, each row links to the corresponding configuration page so you can act on it right away.

### Why it's useful

Think of it as your first stop when checking on a site — especially a new one. In a few seconds you can tell whether it's fully configured and protected, or whether something (a missing certificate, no WAF activity, an unconfigured API schema) needs your attention, without having to visit half a dozen different pages to find out.

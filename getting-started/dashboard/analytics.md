# Analytics

Analytics gives you a real-time view of everything happening on your traffic: how it's being delivered, how well it's cached, whether anything suspicious is going on, and how your DNS is behaving. You'll find it in the left-hand menu under **Insights → Analytics**.

Depending on your plan, you may not see every report listed below. For example, WAF, Bot Management, AntiDDoS, Backends or DNS analytics are only available if your plan includes those services. If a section you expect to see is missing, reach out to your account manager.&#x20;

Analytics is organized into different sections, one per area of your traffic:

* **Delivery** — your overall CDN traffic: requests, bandwidth, cache performance, status codes...
* **Midtier** — traffic between your edge nodes and your mid-tier cache layer.
* **Backends** — how your origin servers are performing.
* **WAF** — web application firewall activity.
* **Bot Management** — bot detection and mitigation activity.
* **AntiDDoS** — traffic legitimacy and blocked attack traffic.
* **DNS** — queries received by your DNS zones.

If you're looking for the same data over a longer period (weeks or months instead of the last few hours), check out the [History](history.md) section instead — it shares the same sections but with a longer retention window.

### Getting around Analytics

Every section of Analytics (Delivery, Midtier, Backends, WAF, Bot Management, AntiDDoS, DNS) shares the same building blocks, so once you're comfortable with one, you're comfortable with all of them.

The filters panel sits at the top of every section and has two tabs: **Quick selection** and **Other filters**. Use the collapse icon in the top-right corner of the panel to hide it and give the charts more room.

**Quick selection**

This is where you decide _when_ and _which sites_ you want to look at:

* **Time range** — pick a preset (**15m**, **30m**, **1h**, **8h**, **24h**, **7d** or **1M**) or choose **Custom** to set your own range.
* **Sites** — open the dropdown to pick one or more of your sites (you can type in the search box to find one quickly). **All** selects every site and **None** clears the selection.

**Other filters**

Need to go further than a time range and a site? The **Other filters** tab lets you narrow the data down by as many conditions as you like. The number next to the tab name tells you how many filters are active.

To add one:

1. Click **Add filter** and choose a field from the list (for example _Country Code_, _HTTP Method_, _IP Address_, _User Agent_, _Referer_, _Cache Status_ or _Status Code_). You can type in the search box to find a field faster. Fields you've already added are marked as such, but you can still use them in another row.
2. Choose whether the filter **includes** or **excludes** that traffic with the **is** / **is not** switch:
   * **is** (green) — show _only_ traffic that matches.
   * **is not** (red) — _hide_ traffic that matches. It's useful, for example, to leave out your own monitoring traffic or a country that isn't relevant to you.
3. Set the value. Depending on the field you'll type it and press Enter (you can keep adding more values with **Add another…**, as in the country codes `ES` or `US`), or pick from a ready-made list of options (for instance GET, POST, PUT... for _HTTP Method_, or Hit, Miss and Pass for _Cache Status_).

You can combine several filters, including the same field more than once. For example, **Country Code is ES** together with **Country Code is not US**. To remove one, click the trash icon (🗑) at the end of its row.

**Applying and resetting**

Nothing changes in the charts until you click **Apply Filters**, so you can set up everything first and update the data in one go. Made a mess? **Reset Filters** puts everything back to its default.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-26 a las 11.03.49.png" alt=""><figcaption></figcaption></figure>

#### Customizing your dashboard

Click the settings icon in the top-right corner to open **Customize Dashboard**. From here you can:

* Toggle any widget on or off.
* Drag widgets to reorder them.
* Use **All** / **None** to show or hide everything in one click.

Your layout is remembered, so you only need to set it up once per report.

#### Sharing a snapshot

Need to send a colleague exactly what you're looking at, filters and all? Click the share icon (⤳) to open **Manage Snapshots**. Creating a snapshot generates a read-only, shareable link with your current filters and visible widgets baked in — it's copied to your clipboard automatically. You can create as many snapshots as you need and revisit them later from the same panel.

<figure><picture><source srcset="../../.gitbook/assets/Captura de pantalla 2026-10-06 a las 14.01.15.png" media="(prefers-color-scheme: dark)"><img src="../../.gitbook/assets/Captura de pantalla 2026-10-06 a las 14.00.13.png" alt=""></picture><figcaption></figcaption></figure>

#### Digging into a table row

Most tables (Countries, IPs, HTTP Methods, User Agents, Content Type, Protocol, Referers...) have a small filter icon on the right of each row. Click it to instantly filter the whole report down to that value — for example, click the filter icon next to a country to see only that country's traffic.

### IP Address table

The IP address table shows the IPs from where the requests are sent. Each row in the table represents a source IP address and provides information about the number of requests, bandwidth consumed, its risk level, and the available actions to manage it.

**IP:** Source IP address of the requests. A colored dot provides a visual indication of the risk level.

**Risk:** Risk score of the IP address (0–100). A score of 0 indicates no known risk.

**Requests:** Number of requests received from the IP address and its percentage of the total traffic.

**Bandwidth:** Volume of data transferred from the IP address and its percentage of the total bandwidth.

**Actions:** Three action buttons: IP details, block/allow, and filter.

#### **IP Information**

Clicking the **IP Information** button opens a modal with a comprehensive analysis of the selected IP address, including geolocation, network information, risk score, and threat analysis.

The **Threat Analysis** section is arguably the most relevant from a security perspective. It consolidates abuse reports, threat intelligence sources, and behavioral flags associated with the IP address.

***

### Delivery

Your main traffic report — the one you'll probably check most often. It answers the basic questions: how much traffic am I getting, how well is it being cached, and is anything going wrong?

**At a glance:** Efficiency score, total traffic, requests per second, and cache ratio (split by requests and bandwidth).

**Charts:** requests, bandwidth, status codes (split into 3xx/5xx so you can spot problems fast), hit ratio, response time, and purges over time.

**Breakdown tables:** which sites and URLs are driving traffic, status codes, cache hit/miss/pass detail, countries (on a map), IP addresses, HTTP methods, user agents, content types, protocols, referrers, debug reasons, and JA3 fingerprints.

**What's "Hit Ratio Details"?** It shows exactly how many requests were served from cache (**Hit**), had to go to your origin (**Miss**), or bypassed the cache entirely (**Pass**) — useful for spotting caching issues quickly.&#x20;

### Midtier

Same idea as Delivery, but for the layer between your edge nodes and your origin. Useful when you want to understand how traffic behaves _before_ it reaches Delivery, especially for multi-tier caching setups.

### Backends

Focused entirely on how your origin servers are performing under the CDN. Beyond requests and bandwidth, you'll find origin-specific detail:

* **Origin** and **ASN** breakdowns — which backends and which networks are generating the load.
* **Cache-Control** and **Vary** header usage — a quick way to check your origin is sending the caching headers you expect.
* Low-level timing metrics: **t-fetch** (time to fetch from origin), **t-process**, **t-berespbody**, **t-beresp** — handy for backend performance troubleshooting.

If you see "No data available", try widening your time range or checking your filters — it usually just means there was no backend traffic in that window. \{% endhint %\}

### WAF

Shows what your Web Application Firewall has been blocking: positives by site, by geographic origin (on a map), which payload rules are triggering, and which URL paths are most targeted. Use the **Reports** button in the top-right to generate a formatted report.

### Bot Management

Shows bot-detection activity: which countries, sites, IPs, URLs, crawlers, and user agents are triggering bot assessments. Uses the same IP table with block/unblock/details actions described above — handy for reacting to unwanted bot traffic on the spot.

### AntiDDoS

Tracks traffic legitimacy at a glance: number of sites, IPs, countries, ASNs and user agents involved, plus a **Legitimate vs Blocked** traffic chart so you can see attack traffic as it happens. Also includes Accept-Language, Accept-Encoding, request paths, and JA3 fingerprints — useful fingerprinting data when investigating an attack.

{% hint style="info" icon="lightbulb-on" %}
**Downloading historical IP data:** click **Reports → Download IP History** to export the IPs seen over a custom date range (up to 31 days). Note that IP history is computed daily, so today's IPs won't be included until tomorrow.
{% endhint %}

### DNS

If you use Transparent Edge for DNS, this report shows queries received per domain, broken down by record type (CNAME, TXT, MX, etc.) with both a table and a donut chart, plus a timeline of query volume. Pairs well with the DNS Delegation configuration section if you're setting up or troubleshooting a DNS zone.

{% hint style="info" %}
Remember that everything you can do from our [dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F), you can do from our [API](https://docs.transparentedge.eu/v/english/guias/api).
{% endhint %}

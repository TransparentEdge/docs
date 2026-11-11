# History

While Analytics is built for watching what's happening right now, **History** is where you go to look back — days, months, even your entire history with Transparent Edge. Same underlying data, longer memory. You'll find it under **Insights → History**.

Like Analytics, History is split into one report per area of your traffic:

* **Delivery** — historical requests, bandwidth, and cache performance.
* **Midtier** — historical mid-tier cache traffic.
* **Storage** — historical storage consumption.
* **WAF** — historical firewall activity.
* **Bot Management** — historical bot detection activity.
* **i3** — historical image processing / transformation activity.
* **Transcoding** — historical video transcoding activity.
* **DNS** — historical DNS query volume.
* **IP Info** — historical IP-related data.

Which reports you see here depends on your plan. If a service isn't part of your contract, its report won't show up in the menu.

### Getting around a report

History shares several elements with Analytics so if you already know how to use Analytics, you'll find History familiar.

#### Choosing your time range

Instead of short presets like 'last 15 minutes', History gives you **Quick Selection** options built for the long view: _Last week, Current month, 3 months, 6 months, Current year,_ or _All history_. You can also pick a custom date range directly, and narrow things down to specific sites. Click **Apply Filters** when you're ready, or **Reset Filters** to start over.

#### Customizing your dashboard

Same as in Analytics: click the icon to open **Customize Dashboard**, where you can show/hide widgets and drag them into the order that makes sense for you.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-26 a las 14.13.25.png" alt=""><figcaption></figcaption></figure>

#### Monthly Consumption

Most History reports include a **Monthly Consumption** table a simple month-by-month breakdown of your usage (e.g. requests). Each row can be downloaded individually or filtered further. This is the easiest way to answer "how much did we use last month?" without digging through charts.

### Delivery

Historical view of your overall delivery traffic: requests per second, bandwidth, status codes (broken down by 2xx/3xx/4xx/5xx), and hit ratio, all over the time range you choose. Use this when you need to compare, say, this month against last month, or pull numbers for a report.

### Midtier

The historical counterpart to Analytics' Midtier report — requests, bandwidth, status codes and hit ratio for your mid-tier cache layer over time.

### Storage

Historical consumption of any storage service tied to your account.

### WAF

Historical view of firewall activity — how many requests were flagged over time, useful for spotting trends (e.g. a gradual increase in attack attempts) that are hard to see in the shorter Analytics window.

### Bot Management

Alongside the usual requests-over-time chart, this report includes an **Assessment Reason** breakdown (as a donut chart): how many requests were flagged as _Positive_, led to a _Show captcha_, were _Captcha passed_, ran in _Bypass mode_, or were _Blocked_ outright — with both percentages and raw counts. Also includes the Monthly Consumption table described above.

### i3

Historical activity for image processing (resizing, format conversion, etc.) — see the i3 section for details on what each transformation does.

### Transcoding

Historical activity for video transcoding jobs — see the Transcoding section for how transcoding works.

### DNS

Historical query volume for your DNS zones, broken down by record type.

### IP Info

Historical log of IP-related lookups and events.

### Limit Alerts (consumption alerts)

Rather than checking History manually to make sure you're not about to go over a limit, you can set up an alert and let Transparent Edge notify you instead.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-26 a las 14.10.18.png" alt=""><figcaption></figcaption></figure>

From any History report, click the alert icon in the top-right corner to go to **Limit Alerts**. From there:

1. Click **New alert**.
2. Set your **Threshold** — the value that, once crossed, should trigger a notification (e.g. a bandwidth limit in Gb).
3. Under **Notifications**, click **Add channel** and choose where you want to be notified.
4. Click **Create alert**.

Once created, your alert appears in the Limit Alerts list, showing whether it's active, its threshold, when it last notified you, and through which channels — and you can edit or remove it at any time.

This is the direct evolution of the old "Consumption Alerts" feature — same purpose (get warned before you go over your limit), now managed from its own dedicated page instead of a button on a single report.&#x20;

{% hint style="info" %}
Remember that everything you can do from our [dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F), you can do from our [API](https://docs.transparentedge.eu/v/english/guias/api).
{% endhint %}

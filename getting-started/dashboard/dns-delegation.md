# DNS Delegation

With **DNS Delegation**, Transparent Edge becomes the DNS provider for your domain. You point your domain's name servers to us and manage all of its DNS records from the dashboard, next to the rest of your CDN configuration.

You'll find it under **Configuration → DNS Delegation**.

DNS Delegation is available if you have it included in your contract. If you don't see it in the menu or want to enable it, contact your account manager or our support team.&#x20;

Once your zone is delegated, Transparent Edge answers every DNS query for your domain with the records you create here. That's also what makes DNS activity show up in Analytics and History, in their **DNS** sections.&#x20;

### How to delegate a domain

Delegating a domain takes four steps:

1. **Add the domain** to the dashboard (see Adding a domain).
2. **Create your DNS records** in the dashboard (see Managing DNS records). Recreate everything your domain needs — website, email, verification records, subdomains... — _before_ the next step.
3. **Change the name servers (NS records) of your domain**, at your registrar or current DNS provider, so they point to the two Transparent Edge name servers:

```
   ns.transparentedge.eu
   ns.transparentedge.es
```

You'll find both at the top of the DNS Delegation page, each with a button to copy it. 4. **Wait for the change to spread.** DNS changes can take a while to reach everyone. When they do, Transparent Edge is the one answering for your domain.

Create your records **before** changing the name servers. From the moment the delegation takes effect, the only records that are answered are the ones that exist in the dashboard: anything missing (such as the MX records for your email) will stop working until you add it.

### Adding a domain

1. Click **New Domain**.
2. In the **Create Domain** window, write the **Domain name** (for example, `example.com`).
3. Click **Create**. You'll see the message _Domain created successfully_, and the new domain is selected automatically, with an empty list of records.

To work with a domain you already added, choose it in the **Select a domain** dropdown of the **DNS Domains** card. To remove the selected domain, click **Delete**; you'll be asked to confirm before it's removed.

### Managing DNS records

When you select a domain, its **DNS Records** appear below. If it has none yet, the page tells you so — _Once the zone is delegated, the records you add here are the ones we answer with_ — and offers **Add your first record**.

The table shows:

| Column          | What it shows                                                            |
| --------------- | ------------------------------------------------------------------------ |
| **ID**          | The record's identifier.                                                 |
| **Name**        | The full name of the record, with a copy button.                         |
| **Type**        | The record type (see below).                                             |
| **Value**       | What the record points to, with a copy button.                           |
| **TTL**         | How long, in seconds, resolvers can keep the answer before asking again. |
| **Description** | Your optional note.                                                      |
| **Actions**     | Edit and Delete.                                                         |

At the bottom you can choose how many **rows per page** to show and turn on **Auto-refresh** to keep the list up to date.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-29 a las 12.51.01.png" alt=""><figcaption></figcaption></figure>

#### Creating a record

1. Click **New DNS Record**.
2.  Fill in the **Create DNS Record** window:

    | Field           | What to enter                                                                                                                                |
    | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
    | **Record type** | The type of record you need (see the table below).                                                                                           |
    | **Name**        | The name of the record, within your domain: the domain itself, a subdomain such as `www.example.com`, or a wildcard such as `*.example.com`. |
    | **Value**       | What the record points to (an IP address, a host name, some text...). It depends on the type.                                                |
    | **TTL**         | How long the answer can be cached: **5 minutes**, **10 minutes** (the default), **1 hour** or **1 day**.                                     |
    | **Description** | Optional. A note to remind you what the record is for.                                                                                       |
3. Click **Create record**.

To change a record later, click **Edit**, adjust what you need and click **Save changes**. To remove it, click **Delete** and confirm when asked.

{% hint style="info" icon="lightbulb-on" %}
If you're about to change a record that is in production, lower its **TTL** first (for example, to **5 minutes**) and wait for the old TTL to expire. That way, once you make the change, it takes effect much faster.
{% endhint %}

#### Record types

| Type                                                              | Use it to...                                                                                                                    |
| ----------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **A**                                                             | Point a name to an IPv4 address.                                                                                                |
| **AAAA**                                                          | Point a name to an IPv6 address.                                                                                                |
| **CNAME**                                                         | Make a name an alias of another host name.                                                                                      |
| **ALIAS**                                                         | Point a name to another host name, in the places where a CNAME isn't allowed (such as the domain itself).                       |
| **CDN**                                                           | Serve a name through Transparent Edge directly. The **Value** field is disabled, because it's managed automatically by the CDN. |
| **MX** (**MX\_1**, **MX\_5**, **MX\_10**, **MX\_20**, **MX\_30**) | Tell where the email for your domain is delivered. The number is the record's priority: lower numbers are tried first.          |
| **NS**                                                            | Publish a name server record.                                                                                                   |
| **SRV**                                                           | Publish the location of a service.                                                                                              |
| **TXT**                                                           | Store text, often used to verify domain ownership or to configure email security (SPF, DKIM...).                                |

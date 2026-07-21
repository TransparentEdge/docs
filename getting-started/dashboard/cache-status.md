# Cache status

Cache Status allows you to check, in real time, the caching behavior of a URL across the entire edge server network. It provides server-by-server visibility into whether a piece of content is being served from cache, has expired, or has never been stored.

#### How to analyze a URL?

At the top of the section is the **Analyze URL** form, made up of three fields:

**Protocol:** Dropdown selector to choose `https://` or `http://`.

**Domain:** Dropdown selector with the domains available for the active account/service (e.g. `example.com`).

**Path:** Free text field to enter the path of the content to analyze (`/path/to/content`).

Next to the fields there are two buttons:

**Reset:** returns to the initial state to perform a new analysis.

**Check Cache status:** runs the cache status query.

{% hint style="warning" %}
The URL entered must be publicly accessible from the internet for the analysis to complete successfully.
{% endhint %}

#### Results summary

After running the analysis, five aggregated indicators are displayed:

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-07-21 a las 11.28.45.png" alt=""><figcaption></figcaption></figure>

| Indicator      | Meaning                                                                                        |
| -------------- | ---------------------------------------------------------------------------------------------- |
| Total Servers  | Total number of edge servers queried.                                                          |
| Cache Hits     | Number of servers that served the content directly from cache.                                 |
| Cache Misses   | Number of servers where the content was not in cache or was not valid.                         |
| Cache Pass     | Number of servers that bypassed the cache (non-cacheable content, or configured in pass mode). |
| Cache Coverage | Percentage of data requests or overall traffic that is served and resolved by a cache.         |

#### Per-server detail

Below the summary, a list appears showing the individual status of each edge server. Each card shows:

* **Result:** cache result status for that server. Possible values:
  * `HIT` – the content was served from cache.
  * `MISS` – the content was not found in cache or was not fresh.
  * `PASS` – the request bypassed the cache.
* **Ttl Remaining Time:** remaining time-to-live (TTL) of the object in cache.
* **Grace Remaining Time:** remaining time of the grace period, during which an expired object can still be served while it is revalidated in the background.
* **Keep Remaining Time:** remaining time the object will be kept stored even after it has expired (useful for serving stale content in case of origin failures).
* **Age Time:** time elapsed since the object was stored in cache.
* **Http Status:** HTTP response code returned by the cached object.
* **Is Stale:** indicates whether the object is expired (stale) but still present in cache.
* **Vary:** determines how to match the headers of future requests to decide whether a cached response can be used instead of requesting a new one from the origin server.
* **Hits On Object:** number of times the object has been served from that server.
* **Hfp:** internal hash fingerprint identifier of the object in cache.

#### Exporting results

The **Download Report** button, located in the top-right corner of the list, lets you download a report with the full results of the analysis.

# Cache status

Is a given piece of content cached, and for how long? **Cache Status** answers that in real time. You enter a URL and Transparent Edge asks every edge node in the network, at once, whether it has that object in cache. For each node you can see if it served the object from cache, how long it will remain valid and what response it would give.

You'll find it under **Cache → Cache Status**.

It's very handy to:

* Check that your content is being cached as you expect.
* Confirm that an invalidation has worked, and that the old copy is gone from every node.
* Understand why something isn't being cached, by looking at the TTL and the result on each node.

### Checking a URL

1. Choose the **Protocol**: `https://` or `http://`.
2. Pick the **Domain**. The list shows the domains of your account; you can type to search.
3. Optionally, write the **Path** of the content you want to inspect (for example, `/images/logo.png`). If you leave it empty, the root of the domain is checked.
4. Check the **Preview** line under the form: it shows the exact URL that will be queried (for example, `https://www.example.com/images/logo.png`).
5. Click the cloud button to run the check. It queries every node in parallel, so results appear in a few seconds.

To start over, click **Reset form** (the circular arrow next to the cloud button).

The URL has to be **publicly accessible from the Internet**. If it isn't, the analysis can't be completed.

Until you run a check, the page shows a short reminder of the three steps — **Pick a URL**, **Query every edge node** and **Review the response** — and what each result means:

* **Hit** — served from the edge cache: the fastest response.
* **Miss** — not in cache, so it was fetched from your origin.
* **Pass** — caching was bypassed (for example, because of private or `no-store` directives).

{% hint style="warning" %}
The URL entered must be publicly accessible from the internet for the analysis to complete successfully.
{% endhint %}

### Reading the results

#### Summary

At the top of the results you'll see a **Cache hit rate** with a bar that shows, at a glance, how the nodes split, and four counters:

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-27 a las 17.23.51.png" alt=""><figcaption></figcaption></figure>

| Indicator | What it means                                            |
| --------- | -------------------------------------------------------- |
| Nodes     | How many edge nodes were queried.                        |
| Hits      | Nodes that served the object from cache.                 |
| Misses    | Nodes where the object wasn't in cache, or wasn't valid. |
| Passes    | Nodes that bypassed the cache.                           |

A low hit rate isn't always a problem. If the content was just published, or you've just invalidated it, the nodes haven't requested it yet, and the first visitors served by each node will warm up its cache. Run the check again after a few requests to see how it evolves.

#### Filtering the nodes

Under the summary, use the tabs to choose which nodes to list: **Nodes** (all of them), **Hit**, **Miss** or **Pass**. The number in brackets tells you how many nodes there are in each group — handy for quickly finding the ones that don't have your content in cache.

#### Details of each node

Every node has its own card, with its name (for example, `edgeserver-1-199`), its result (**HIT**, **MISS** or **PASS**) and these details:

| Field     | What it means                                                                                                                          |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Age**   | How long ago the object was stored in cache.                                                                                           |
| **TTL**   | Time left before the object expires and has to be refreshed (_time to live_).                                                          |
| **Hits**  | How many times that node has served this object.                                                                                       |
| **HTTP**  | The HTTP status code of the cached response.                                                                                           |
| **Stale** | Whether the object has expired but is still kept in cache (**Yes** / **No**).                                                          |
| **Grace** | Time left in the _grace period_: while it lasts, an expired object can still be served while a fresh one is fetched in the background. |
| **Keep**  | How long the object is kept after expiring. It's useful to be able to serve stale content if your origin fails.                        |

#### Downloading the results

Click **Download Excel** to download the full analysis as a spreadsheet, for example to share it with your team or attach it to a support request.

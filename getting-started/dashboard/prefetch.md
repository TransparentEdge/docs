# Prefetching Cache

With **Prefetching cache**, you tell Transparent Edge which content to load into the cache of every edge node _before_ your visitors ask for it. This is known as _warming up_ the cache: when the first visitor arrives, the content is already there, so they get the fast response instead of waiting for it to be fetched from your origin.

You'll find it under **Cache → Prefetching cache**.

It's very useful when:

* You're about to launch something and expect a lot of traffic at once (a campaign, a release, a live event).
* You've just invalidated content and want the new version ready in every node before visitors request it.
* You have heavy content (large images, video segments, downloads) that you don't want your origin to serve on the first visit.

### Prefetching URLs

1. Click **New Prefetch**.
2. In the **Prefetch URLs** window, write the URLs you want to warm up in the **List of URLs** box, **one URL per line**:

```
   https://www.example.com/path/to/content
   https://www.example.com/path/to/another/content
   https://www.example.com/path/to/another/content/subcontent
```

3. Click **Prefetch**. **Discard** closes the window without doing anything.

Write each URL in full, starting with `https://` or `http://`. Don't use spaces or commas to separate them: only line breaks. If a line isn't a valid URL (for example, `www.example.com` without the protocol), the box turns red and shows _Invalid URL found_, and nothing is sent until you fix it.&#x20;

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-29 a las 13.28.36.png" alt=""><figcaption></figcaption></figure>

### The Prefetching page

The page lists the prefetch requests you've made. For each one it shows:

| Column      | What it shows                                                                                                               |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| **ID**      | The identifier of the prefetch.                                                                                             |
| **Date**    | When it was requested, with day and time.                                                                                   |
| **User**    | The email of the user who requested it.                                                                                     |
| **URL**     | The URL that was prefetched, with a button to copy it. Long URLs are shortened; the copy button gives you the full address. |
| **Actions** | **Refetch** runs the prefetch again for that URL.                                                                           |

Each row corresponds to one URL, and the most recent ones appear first.

#### Repeating a prefetch

If you need to warm up the same URL again (for example, after invalidating it), you don't have to type it again: click **Refetch** in its row.

At the bottom of the list you can choose how many **rows per page** to show, see the total number of records and the time of the last update, turn on **Auto-refresh** to keep the list up to date, and move between pages.

The list keeps the history of your previous prefetches. If you haven't requested anything yet, the page says _No cache warm-ups yet_ and offers **Warm up your first URLs**, which opens the same window as **New Prefetch**.

### Prefetching through the API

You can also request a prefetch from your own scripts or deployment pipelines, with a `POST` request that carries the list of URLs:

```
POST /v1/companies/<company_id>/prefetch/
```

json

```json
{
  "urls": [
    "https://www.example.com/path/to/content",
    "https://www.example.com/path/to/another/content"
  ]
}
```

Replace `<company_id>` with your company ID (for example, `c123`).

In case of doubts, please reference our [API documentation](https://api.transparentcdn.com/docs).

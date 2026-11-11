# Invalidation

Invalidating (or purging) is as important as caching. When you update something on your origin, you need the CDN to drop the old cached copy right away instead of waiting for it to expire or be evicted. Once an object has been purged, the next request for it is fetched from your origin instead of from the cache.

You'll find it under **Cache → Invalidation**. You can invalidate content in two ways, switchable via the tabs at the top of the page:

* **By URL** — target one or more specific URLs.
* **By TAG** — target every cached object that shares a given cache tag, regardless of its URL.

Whichever you choose, you also decide _how_ the content is invalidated. There are four methods, explained in Invalidation methods below: **Purge**, **Soft Purge**, **Ban** and **Warm up cache**.

Everything you can do from the dashboard is also available through the API, so you can automate your invalidations.&#x20;

<figure><img src="../../.gitbook/assets/Captura de pantalla 2026-10-26 a las 12.09.45.png" alt=""><figcaption></figcaption></figure>

### Purging by URL

1. Click **+ Purge** in the top-right corner, then choose **Purge URLs**.
2.  A wizard walks you through the available invalidation methods. For each URL you want to invalidate, fill in:

    * **Protocol** (`http://` or `https://`)
    * **Domain** — pick from the sites associated with your account
    * **Path** — the path to the specific content (leave it empty to purge the whole domain)

    A live **Preview** below each row shows you the exact URL that will be purged, so you can double-check before submitting.
3. Need to purge several URLs at once? Click the **+** button to add another row, or the trash icon to remove one.
4. Choose your invalidation method and click **Purge** to confirm.

For example, these are all valid URLs to invalidate:

```
https://www.example.com/myobject.js
https://www.example.com/myobject.js?v=2.0
https://www.example.com/img/image.jpg
```

Each one is treated as a complete URL, so `myobject.js` and `myobject.js?v=2.0` are two different objects and have to be invalidated separately.

The protocol is required, but it doesn't matter whether you use `http` or `https`: the object is invalidated either way.

**Advanced mode**: if you already have a list of full URLs copied somewhere (for example, in your clipboard), turn this on to paste them all in at once instead of building each one field by field.

### Purging by TAG

1. Click **+ Purge**, then choose **Purge TAGs**.
2. In the **Cache tags** box, enter the tags you want to invalidate, one per line (no spaces or commas allowed).
3. Choose your invalidation method and click **Purge** to confirm.

This invalidates every object currently carrying that tag, no matter which URL it's cached under. It saves you from writing scripts that build the list of affected URLs every time your content changes, and it's a good alternative to broad Ban patterns.

#### How tagging works

For tag-based invalidation to work, your content needs to be tagged in the first place. This is done via the `Surrogate-Keys` response header, which can hold one or more tags for a given object.

When a visitor requests an object that isn't cached yet (a _miss_), the CDN fetches it from your origin and checks the response for the `Surrogate-Keys` header. If it's there, its value is assigned to the object as tags. If the value contains several words separated by spaces, **all of them** become tags of that object, so you can invalidate it using any of them. For example, this response:

```
HTTP/1.1 200 OK
Surrogate-Keys: main statics1
Content-Type: text/html
```

labels the object with `main` and `statics1`. Invalidating either tag invalidates this object and every other object that carries it, whatever their URLs.

Tags are assigned when the object is fetched from your origin, so only objects cached _after_ your origin starts sending the header carry the tags. Objects that were already in the cache won't have them until they're fetched again.

#### Tags and objects: a many-to-many relationship

This is what makes tag invalidation so flexible: one object can have several tags, and one tag can be shared by many objects. Take these two responses from your origin:

```
GET /shop/
Surrogate-Keys: shop main-shop

GET /shop/product/24656-tshirt
Surrogate-Keys: shop product-tshirt
```

There are two objects and three tags. `main-shop` belongs only to the shop page, `product-tshirt` only to the t-shirt page, and `shop` to both. So you can purge just the shop home page (`main-shop`), just one product (`product-tshirt`), or everything in the shop at once (`shop`), without building a list of URLs.

#### Assigning tags

There are two ways to do it:

* **From your origin (recommended).** Add the `Surrogate-Keys` header in your application or in the web server that serves the content. This keeps your caching logic close to your application code. For example, with Nginx:

```
  location ~* ^/content/.*\.(css|js|png|gif)$ {
      add_header Surrogate-Keys "statics content";
  }
```

This tags every `css`, `js`, `png` and `gif` file under `/content/` with the tags `statics` and `content`.

* **From the CDN, using VCL.** If you need more control without touching your origin, add the tags in the `vcl_backend_response` subroutine:

```
  sub vcl_backend_response {
      if (bereq.url ~ "\.(jpg|jpeg|png|gif)($|\?)") {
          set beresp.http.Surrogate-Keys = "images " + beresp.http.Surrogate-Keys;
      }
  }
```

This adds the tag `images` to every resource with the extension `jpg`, `jpeg`, `png` or `gif`.

In the VCL example, keep the space after the tag (`"images "`) before adding the existing header. If your origin already sends other tags in `Surrogate-Keys`, they won't work without it.&#x20;

#### Purging by TAG with the API

You can also invalidate by tag from your own scripts. Make an authenticated `POST` request to `/v1/companies/<company_id>/tag_invalidate/` with a body that contains the key `tags`:

```
// tags.json
{
    "tags": [
        "main",
        "rss"
    ]
}
```

```
curl --request POST --data @tags.json \
  -H "Authorization: Bearer <API_TOKEN>" \
  -H "Content-Type: application/json" \
  "https://api.transparentcdn.com/v1/companies/<company_
```

### Invalidation methods

Both URL and TAG invalidations use the same four methods:

| Method              | What it does                                            |
| ------------------- | ------------------------------------------------------- |
| **Purge** (default) | The standard way of invalidating an object.             |
| **Soft Purge**      | Marks the object as expired instead of removing it.     |
| **Ban**             | Invalidates every cached object matching a pattern.     |
| **Warm up cache**   | Purges the object and immediately fetches a fresh copy. |

#### Purge

This is the standard way of invalidating an object. It requires the complete URL, and you can invalidate several URLs in a single request.

The object is **removed from the cache immediately**, and the next request fetches it from your origin.

#### Soft Purge

Like Purge, it works on the complete URL, but with one notable difference: the object isn't removed right away. It is flagged as **expired (stale)** instead. The next request is served with the cached object, while a background process fetches an updated copy from your origin. If the update succeeds, the old object is replaced.

This is very useful to protect yourself against origin failures. If your origin has issues and can't serve content, a regular Purge would leave the CDN with nothing to deliver, and visitors would get a `503` error. With a Soft Purge, the CDN keeps returning the cached (expired) object instead, provided it's actually in the cache. You decide which behaviour suits each case better.

**API tip:** to turn a purge into a soft purge, add `"soft": true` to your payload.&#x20;

#### Ban

A Ban invalidates content using a **pattern**, similar to a regular expression, instead of a single URL. It's recursive: it affects every cached object that matches.

For example, to invalidate all the images under `/content/img/`, you would request a Ban with the pattern:

```
https://www.example.com/content/img/
```

You still need to include the protocol and the domain.

{% hint style="warning" %}
Be very careful with Bans. A Ban at the root of your site (`https://www.example.com/`) invalidates the **entire** content of your cache, and every subsequent request goes straight to your origin, which can put it under heavy load and noticeably reduce your cache efficiency. Use other methods whenever you can, such as invalidation by TAG.&#x20;
{% endhint %}

#### Warm up cache

It works like Purge, with one extra step: once the object has been invalidated, the CDN automatically makes a request to the same URL to **cache a fresh copy**. That way, the next visitor doesn't hit a cold cache.

{% hint style="info" %}
**API tip:** to warm up the cache when invalidating, add `"refetch": true` to your payload.&#x20;
{% endhint %}

### Reviewing past invalidations

Every invalidation — by URL or by TAG — is logged automatically, on its respective tab. Each entry shows the **ID**, **Date**, **User** (who requested it), the **URLs** or **Tags** involved, the **Type** (Purge/Soft Purge/Ban), whether it was **Refetch**ed (warmed up) afterwards, and an action to repeat the same invalidation with one click.

Useful settings at the bottom of the table:

* **Rows per page** — control how many entries are shown at once.
* **Auto-refresh** — turn this on to have the table update automatically as new invalidations come in (handy if a teammate is purging content at the same time).

This audit trail — knowing exactly who purged what and when — is new. Previously there was no way to see this from the dashboard itself.&#x20;

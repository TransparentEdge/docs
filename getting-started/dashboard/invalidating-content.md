# Invalidating content

Invalidating (or purging) is as important as caching. Purging removes the object from the cache before it expires or is evicted.&#x20;

Once an object has been purged, a subsequent request will fetch it from the source instead of from the cache.

Here, we'll explain how to invalidate using four methods:

* Simple invalidations (PURGES)​
* Soft invalidations (SOFTPURGES)​
* Recursive invalidations (BANS)​
* Simple + Warm Up​

There is another type of invalidation that leverages on tags attached to the objects and is explained here.

Remember that everything that can be done on our dashboard is available via API for automation.

To create an invalidation, log in to our dashboard and go to:

Invalidation -> Purge -> Purge URLs

A wizard will be displayed explaining the different invalidation methods available. You can click on "Advanced mode" to paste a list of URLs directly or complete them using the wizard.

### Simple invalidations (PURGES)

This is the standard way of purging an object. A full URL is required, and you can invalidate multiple URLs in a single request, for example:

`https://www.example.com/myobject.js`

`https://www.example.com/myobject.js?v=2.0`

`https://www.example.com/img/image.jpg`

Protocol is required but it does not matter whether it is http or https.

This method immediately removes the object from the cache, and the subsequent request will fetch it from the source (your original backend).

### Soft invalidations (SOFTPURGES)

This method purges by complete URL, like the PURGE method, but it has one notable difference: objects are not immediately purged from the cache, instead they are flagged as expired (or stale). The subsequent request will serve the cached object, and a background process will update the object in the cache from the source. If the update succeeds, the old expired object will be replaced.

This method is very useful if you want to protect yourself against backend failures. If your backend has issues and is unable to serve content and you PURGE an object, the CDN will return a 503 error. If you use SOFTPURGE instead of an error, it will return the cached yet expired object (provided that the object is actually in the cache). You decide.

{% hint style="info" %}
**API tip**: to transform a purge into a softpurge, add `{..., "soft": true}` to your payload.
{% endhint %}

### Recursive invalidations (BANS)

This method uses a pattern to invalidate content which is similar to a regular expression.

For example, if you wanted to invalidate all the images in /content/img/, you would request a BAN invalidation with the following pattern:

`https://www.example.com/content/img/`

You still need to incorporate the protocol and domain.

Be very careful with BANs. A BAN at the root of your site: `https://www.example.com/` invalidates the full content of the cache, and all subsequent requests will go directly to your backend.

We encourage you to use other available methods, including invalidation by tags.

### Simple + Warm Up

This method behaves in the same way as the PURGE method, the difference is that after the object is invalidated from the cache a request is made to the same URL automatically to warm the cache with a fresh object.

{% hint style="info" %}
**API tip**: to transform a purge into a purge + warm up, add `{..., "refetch": true}` to your payload.
{% endhint %}


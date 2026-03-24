# Default Functions

## [On the client side](https://varnish-cache.org/docs/trunk/users-guide/vcl-built-in-subs.html)

## VCL Subroutines

Built-in subroutines called at key points in the request lifecycle. Override them to control caching, routing, and response behaviour.

**Request flow:** `vcl_recv` → `vcl_hash` → `vcl_hit` / `vcl_miss` → `vcl_backend_fetch` → `vcl_backend_response` → `vcl_deliver` (or `vcl_synth` at any point)

### vcl\_recv

Called at the start of every incoming request, before any cache lookup. Use it to normalise the request, strip unnecessary headers, or decide whether the request should bypass the cache.

You can also assign a [backend](../../getting-started/dashboard/auto-provisioning/backends.md) here or do it in the `vcl_backend_fetch` subroutine.

### vcl\_hash

Called after `vcl_recv`, immediately before the cache lookup.&#x20;

Add data to the hash to create separate cache entries for the same URL, for example, per-host or per-device-type variations.

> **Warning:** Every unique hash key is a separate cache object. Adding a high-cardinality value (e.g. a session token) will produce one cache entry per user, effectively disabling caching for that resource.

### vcl\_miss

Called after a cache lookup that found no valid object. At this point, the request will continue to the backend to retrieve the object.

### vcl\_hit

Called after a successful cache lookup. The cached object is available. Normally does nothing; override it to force a re-fetch under certain conditions

### vlc\_deliver

Called just before the response is sent to the client. Use it to add, remove, or rewrite response headers. The object has already been fetched or retrieved from cache at this point.

### vcl\_synth

Called when a `return(synth(status, reason))` action is issued from any other subroutine (like our `call redirect_request;` action).

## On the origin side

### vcl\_backend\_fetch

Called before Varnish sends a request to the origin. This is the correct place to select a backend and modify backend request headers. Only runs on cache misses and pass requests, cache hits never reach this subroutine.

### vcl\_backend\_response

Called when the backend response headers arrive. Use it to set TTLs, override `Cache-Control`, remove headers that would prevent caching, or discard a bad response.



{% hint style="warning" %}
The rest of the predefined functions in Varnish Enterprise Plus are not allowed to be modified by the user in Transparent CDN, to ensure the security and stability of our clients' sites at all times. However, you can check them out [here](https://varnish-cache.org/docs/trunk/users-guide/vcl-built-in-subs.html).
{% endhint %}

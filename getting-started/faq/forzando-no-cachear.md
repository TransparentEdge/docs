# Forcing No-Cache

## Just what we all love: Sending headers from the origin

At Transparent Edge we want our customers to have as much autonomy as they can. That's why we always encourage a CDN configuration that's as simple as possible and for cache headers to be sent from the origin servers whenever possible. This [link](https://docs.transparentedge.eu/guias/configurar-mis-servidores-para-enviar-cabeceras-de-cache) contains information on how to do it.

To force an object to not be cached, you can configure the Cache-Control header with any of these values:

```
no-cache
no-store
max-age=0
private
must-revalidate
```

Although there are small differences in each one, they have the same effect in practice which is for the content not to be cached. If we had to choose one, we’d use **no-cache**.

## Not caching or bypassing the cache of the CDN

The following covers the majority of use cases for not caching or bypassing the cache of the CDN.

#### Bypass or ignore the cache of the CDN

With this option we can ignore the cache and go directly to the origin to retrieve a fresh object always. Note that this doesn't prevent the object from entering in the cached after the backend response is executed, but it ensures that the client will never receive a cached response. Just call `bypass_cache` to ignore, skip or bypass the cache.&#x20;

```javascript
sub vcl_recv {    
    if (req.http.host == "www.transparent.com" && req.url ~ "^/admin") {
        call bypass_cache;
    }
} 
```

#### Don’t cache the object in the CDN by rewriting the origin headers

```javascript
sub vcl_backend_response {    
    if ((bereq.http.host == "www.transparent.com") && (bereq.url ~ "/my-new-url")) {
            unset beresp.http.Cache-Control;
            set beresp.http.Cache-Control = "max-age=0";
            set beresp.ttl = 0s;
    }
} 
```

With this code, we rewrite the headers that come from the origin and add the amount of time we want the object stored in the cache. In our case, it is 0s—both for the internal TTL which will force the object to be stored in the Transparent Edge cache, and for the cache header to be sent to the browser (Cache-Control).

You can also get more details about rewriting headers [here](https://docs.transparentedge.eu/v/english/getting-started/faq/funcionalidades/reescritura-de-cabeceras).

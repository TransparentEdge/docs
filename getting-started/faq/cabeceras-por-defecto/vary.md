# Vary

The Vary header can be very useful, but sometimes dangerous. It is used to tell caches (either the browser cache, a proxy, or a CDN like Transparent Edge) to save different versions of the same object depending on a given header. Let’s look at some examples of how to use it well and what not do to.

### Bad use of the Vary header

```
HTTP/1.1 200 OK
Vary: Cookies
Cache-control: max-age=86400
Content-Type: text/html; charset=UTF-8
TSSecure: SecureLayer
client-id: 4
TP-l2-Cache: MISS
X-Device: desktop
Date: Mon, 19 Oct 2015 15:24:05 GMT
Age: 85783
Connection: keep-alive
TP-Cache: HIT
```

In the response headers in this example, we can see the Vary: Cookies header. This tells the caches to save a different version of the same object—like an image or a homepage—for each different cookie received. In other words, this type of request would have a very low hit ratio, possibly below 10%, rendering the cache system ineffective.&#x20;

Another bad use example is the Vary: User-agent header, which would save a different version of the same object for every user-agent that visits the site. Be warned, there are quite a few user-agents out there!

### Good use of the Vary header

```
HTTP/1.1 200 OK
Vary: Accept-Encoding
Cache-control: max-age=86400
Content-Type: text/html; charset=UTF-8
TSSecure: SecureLayer
client-id: 4
TP-l2-Cache: MISS
X-Device: desktop
Date: Mon, 19 Oct 2015 15:24:05 GMT
Age: 85783
Connection: keep-alive
TP-Cache: HIT
```

Here we are saving a different version of an object based on whether or not the object is compressed. This is done to maintain backward compatibility with old browsers and is the typical example of using this header well.

Another example is the Vary: X-Device header. When we use Vary this way, we tell Transparent Edge to save a different version of the object based on the X-Device value, which can be desktop, mobile, or tablet.

You can read more about detecting mobile devices [here](https://docs.transparentedge.eu/v/english/getting-started/faq/funcionalidades/deteccion-de-dispositivos).

Learn more about this header[ here](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

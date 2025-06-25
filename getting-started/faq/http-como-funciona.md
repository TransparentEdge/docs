# HTTP, How does it work?

Cache systems like Transparent Edge can provide countless benefits to your website, like reducing load times of pages, absorbing high traffic volumes, improving the security of portals, and sometimes even reducing infrastructure costs.

However, like everything in this world, this one also has its difficulties, although once you understand how a cache works and the flow of HTTP requests, it’s relatively straightforward. In this entry, we’ll explain how an HTTP request works internally, analyze request and response headers, and show you how they get interpreted by an HTTP cache.

![](../../.gitbook/assets/Flujo-HTTP-1.png)

The figure above shows the flow of a website request for an HTTP object from a conventional client to the web server that hosts the site www.transparentedge.eu (without passing through a cache). Not too complicated, right? Let’s take a deeper look.

After establishing the TCP connection with the server—generally over port 80—the client sends an HTTP GET method request for the “/” object (this “slash” object is the site’s homepage; if the client wanted a different object, instead of “/” you would see, for instance, “/categories” or “/image.png”).

The client sends the GET request with a set of headers, all of which are optional except the Host header. The Host header is used so that servers configured for multiple domains know which of their websites needs to be served to the client. Without this header, the server would understand that it had to serve the default website. As mentioned, the rest of the headers are optional, although modern browsers typically send a good deal of information in request headers. These often include the User-Agent header, which identifies the type of browser used for the request; the Cookie header, which contains information stored on the user's browser that is put to multiple uses by the website; and the Referer header, which tells the server the address of the page making the request.

In the response to this request, the web server sends the content itself along with another set of headers, some of which are rather important to the subject at hand.&#x20;

On the first line, we see that the server responds with “HTTP/1.1 200 OK”. Here the server is using code 200 to tell the client that the requested resource has been found and served correctly. Section 10 of the[ RFC2616](https://tools.ietf.org/html/rfc2616#section-10) defines all the status codes that a web server can return in response to an HTTP request. Second, we see the Cache-Control header, which can be configured in the web server or in code. It tells the client’s browser (and all the caches the request passes through) that the object must be updated every 2592000 seconds in this case—or in other words, that the object can be stored on the browser’s cache for 30 days. To keep this brief, we’re not going to analyze the rest of the headers; instead, we’ll see what would happen if this request did pass through a cache like Transparent Edge. Let’s take a look:

![](../../.gitbook/assets/Flujo-HTTP-2.png)

This might appear to complicate things, but it's actually simpler than it seems. To help you understand it, we’ll follow the request through all its steps.

First—and this step is always the same—the client or web browser makes an HTTP request. But instead of going directly to the web server, in this case it's going to pass through a cache like Transparent Edge. This step is completely transparent to the web client, because it doesn’t actually know whether the request is going directly to a web server or to a cache. So: The request is received by the cache system and the cache checks whether or not the object being requested by the client is available in the cache. If it is, the object is returned without having to request it from the web server. If it isn’t, as is the case in our example, the cache will request the object from the web server, resending the exact same request it received from the client (step 2).

At this point, the web server receives the request, processes it, and sends the requested content to the cache along with the response headers (step 3).&#x20;

The cache has just received the content from the web server and this is where the magic of caching occurs: the cache inspects the headers of the HTTP response and reads the Cache-Control header where the server tells it to store the object for 2592000 seconds (30 days). This means that during that time, every time the cache receives a request for that object it won’t have to get it from the web server and can instead serve it directly from the cache. This improves the loading speed of the pages and reduces the work of the web server.

At this point, the cache system adds another set of headers, like the Age header, which tells the client how many seconds the object has been stored in the cache, and the TP-Cache or TP2-Cache headers, which are exclusive to Transparent Edge and tell the browser whether the object has been served from the first cache (TP-Cache: HIT), from the second cache (TP2-Cache: HIT), or from no cache (TP-Cache: MISS and TP2-Cache: MISS) in which case the object had to be requested from the web server.&#x20;

Finally, in the fourth step, the cache sends the content to the client, which interprets and displays it.

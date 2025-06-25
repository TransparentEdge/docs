# Cache key

A cache key is how the CDN saves an object to its cache. By default, the cache key is made up of:

```
[Host header] + [url] + [Query Stream]
```

From there, we can complicate the cache key as much as we wish, adding more elements to save more versions of the same object. Let’s look at an example:

```
https://www.transparentedge.eu/index.html?param=1
​
https://www.transparentedge.eu/index.html?param=2
```

For Transparent Edge—and generally, for any other system of HTTP caches—these are distinct objects. Therefore, even if the first one is cached, it will download again to the origin when the second one is requested because in the CDN cache they are distinct objects.

We can use the[ Vary](https://docs.transparentedge.eu/v/english/getting-started/faq/cabeceras-por-defecto/vary) header to modify the behavior and save more versions of the same object, as previously mentioned. For instance, with an object like this:

```
https://www.transparentedge.eu/index.html?param=1
```

And a Vary header like this:

```
Vary: User-Agent
```

We can make the CDN cache save as many objects as the number of user-agents requesting them.

In the previous example, the cache would save one object for each distinct user-agent (there are many). This would render the cache system ineffective. That’s why we don’t recommend having a Vary header with a user-agent or cookie value. It could be interesting, however, to have an X-Device value, to be able to serve different versions of an object depending on the type of device requesting the resource.\

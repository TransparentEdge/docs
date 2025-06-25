# Cache headers

Cache headers are key to the correct functioning of a cache system in general, and of Transparent Edge in particular. Using them significantly increases the load speed of web pages, improving the user experience.

Transparent Edge uses several types of headers to know whether or not to store an object in its cache, but in this document, we're going to focus on the **Cache-Control** header.

At Transparent Edge, the **Cache-Control** header takes preference over all other cache headers—so if there are more than one, it’s the only one we’ll pay heed to.

The **Cache-Control** header is an improvement in HTTP/1.1 which simplifies the previous caching mechanisms in the HTTP/1.0 protocol. We use this header to indicate to intermediate caches and the browser cache whether or not an object should be stored and for how much time.

Like all HTTP headers, **Cache-Control** is a key-value pair, where the key is always the word **Cache-Control**. In terms of the server’s response, this value can be:

#### **public**

The **Public** header indicates to the cache that the object is public and therefore cacheable. It’s not typically found alone in the Cache-Control header.

#### **private**

Unlike Public, the **Private** header indicates that the content should not be cached because it is user-personalized content and should only be stored on the client's local device.

#### **no-cache**

The **No-Cache** header prevents an object from being cached in Transparent Edge. Even if the object is stored, it is never used.

#### **no-store**

This option prevents the cache from storing—and therefore caching—the object.

{% hint style="info" %}
The Private, No-Cache, and No-Store headers all have the same effect on browsing (with slight differences): They force the content to be cached on the client’s origin servers.
{% endhint %}

#### **must-revalidate**

This option forces the cache to go to the origin for a new copy of the object, telling it to disregard the rest of the cache headers and go get a new copy.

#### **proxy-revalidate**

This works just like **Must-Revalidate** but only affects proxies.

#### **max-age**

This header is used to indicate to Transparent Edge how many seconds we want the object to be stored in the cache and marked as valid. Once this time has passed, the object is marked as expired and the system is forced to go to the client’s origin servers for a new copy. This option affects both proxies and browser caches.

#### **s-maxage**

This is exactly the same as Max-Age, except it only affects the behavior of proxies and not browser caches. The two can be used in conjunction to create special behaviors, for example:

```
Cache-Control: max-age=0, s-maxage=3600
```

In this special case, the object will be stored on Transparent Edge for one hour (3600 s), but it won’t be stored in the browser.

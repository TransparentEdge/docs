# Invalidating methods

There are two methods for invalidating content: **PURGE**, which is a single invalidation that invalidates one particular object; and **BAN**, which is a recursive invalidation that can invalidate more than one object at a time.

## Cache Key

A cache key is how the CDN saves an object to its cache. By default, the cache key is made up of:

```
[Host header] + [url] + [Query Stream]
```

From there, we can complicate the cache key as much as we wish, adding more elements to save more versions of the same object. Let’s look at an example:

```
https://www.transparentcdn.com/index.html?param=1

https://www.transparentcdn.com/index.html?param=2
```

For Transparent Edge—and in general for any other system of HTTP caches—these are distinct objects. Therefore, even if the first one is cached, it will download again to the origin when the second one is requested.

We can use the[ Vary](https://docs.transparentedge.eu/v/english/getting-started/faq/cabeceras-por-defecto/vary) header to modify the behavior and save more versions of the same object. For instance, with an object like this:

```
https://www.transparentcdn.com/index.html?param=1
```

And a Vary header like this:

```
Vary: User-Agent
```

We can make the CDN cache save as many objects as the number of user-agents requesting them.

{% hint style="warning" %}
This example in particular makes the efficiency of the cache decline drastically, which is why we don’t recommend having a Vary header with a user-agent or cookie value. It could be interesting, however, to have an [X-Device](https://docs.transparentedge.eu/v/english/getting-started/faq/cabeceras-por-defecto/x-device) value, value, to be able to serve different versions of an object depending on the type of device requesting the resource.
{% endhint %}

## Single or PURGE invalidations

This kind of invalidation lets you invalidate one particular object in the cache. To do so, you must be sure to enter the full URL with all the parameters included.

{% hint style="info" %}
If you have modified the behavior of the cache-key with the Vary header, you typically shouldn’t have to worry. This kind of invalidation should invalidate all the existing versions of that particular object.
{% endhint %}

One type of PURGE invalidation is known as a SOFTPURGE, which is much less aggressive.

To do an invalidation from our[ dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F), just follow this[ guide](https://docs.transparentedge.eu/v/english/getting-started/dashboard/invalidating-content).

## Recursive or BAN invalidations

BAN invalidations let you invalidate cache content recursively, by sending a blacklist and telling the cache to invalidate any object that matches it. For instance, we can tell it to invalidate **https://www.transparentedge.eu/images/.** This would invalidate any object that matches this URL, such as:

**https://www.transparentedge.eu/images/image1.png** or **https://www.transparentedge.eu/images/2020/02/22/index.php.**

To do an invalidation from our[ dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F), just follow this[ guide](https://docs.transparentedge.eu/v/english/getting-started/dashboard/invalidating-content).

{% hint style="warning" %}
These invalidations are incompatible with the refetching feature.
{% endhint %}

{% hint style="danger" %}
This kind of invalidation can be very dangerous in some cases. For instance, if you invalidated the website https://www.transparentedge.eu/ recursively, you could run into serious problems on your origin platform, as there would soon be a flood of traffic coming from all your users. Use it wisely.
{% endhint %}

{% hint style="info" %}
Remember that everything you can do from our[ dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F) you can do from our[ API](https://docs.transparentedge.eu/v/english/getting-started/faq/glosario/api).
{% endhint %}

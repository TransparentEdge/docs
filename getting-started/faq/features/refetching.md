# Refetching

Refetching is a function through which, after content is invalidated via a PURGE, you force the object to be requested again from the content-invalidation agent running on the CDN nodes.&#x20;

That way, when a real user request comes in, that object will already be in the cache and you won’t have to go get it from the origin.

You can do a refetch via[ API](https://api.transparentcdn.com/docs) or from our[ dashboard](https://dashboard.transparentcdn.com/), as explained in this[ link](https://docs.transparentedge.eu/getting-started/dashboard/invalidando-contenido).

{% hint style="warning" %}
The BAN invalidation method is not compatible with refetching.
{% endhint %}

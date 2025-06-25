# Default Functions

## [On the client side](https://varnish-cache.org/docs/trunk/users-guide/vcl-built-in-subs.html)

### vcl\_recv

This function is called when a new request enters. From this function, you can modify the behavior or content of the request and decide how to process it.&#x20;

In this function, you should configure a [backend](../../getting-started/dashboard/auto-provisioning/sites.md) to which the request should be sent.



vcl\_(recv|miss|backend\_fetch|backend\_response|hash|deliver)

### vlc\_hash

Called after vcl\_recv. In this function, the caching behavior is modified when storing the object in the cache.

### vcl\_miss

It is called immediately after looking up the object in the cache and only if it has not been found.

### vlc\_deliver

This function is called before sending the object to the client.

## On the origin side

### vcl\_backend\_fetch

This function is called before sending the request to the backend to retrieve the object.

### vcl\_backend\_response

This function is called when the backend response arrives successfully.



{% hint style="warning" %}
The rest of the predefined functions in Varnish Enterprise Plus are not allowed to be modified by the user in Transparent CDN, to ensure the security and stability of our clients' sites at all times. However, you can check them out [here](https://varnish-cache.org/docs/trunk/users-guide/vcl-built-in-subs.html).
{% endhint %}

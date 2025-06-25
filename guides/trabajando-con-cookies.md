# Working with cookies

In Transparent Edge, we implement [Varnish Enterprise](https://www.varnish-software.com/es/), allowing you to take advantage of all the benefits it offers.&#x20;

In this case, we implement the [cookieplus](https://docs.varnish-software.com/varnish-enterprise/vmods/cookieplus/) module of Varnish.

This module enables you to interact with both request and response cookies. You can add, remove, and retrieve the values of the Cookie header (req.http.Cookie) and the Set-Cookie header (resp.http.Set-Cookie).

All functions maintain an internal state of the Cookie and Set-Cookie until you release it in the configuration using the **write()** or **setcookie\_write()** function.

Operations with set-cookies can only be used where the [resp or beresp](../config/vcl/vcl-objects.md) objects are available (vcl\_deliver or vcl\_backend\_response).

### Cookie filtering

```javascript
sub vcl_recv
{
    // Keep these Cookies
    cookieplus.keep("JSESSIONID");
    cookieplus.keep_regex("^BNES_SSESS");
    cookieplus.keep_regex("^SSESS[a-zA-Z0-9]+$");
    cookieplus.keep_regex("^SESS[a-zA-Z0-9]+$");

    // Any Cookie that is not kept will be removed
    cookieplus.write();
}
```

### Set-cookies filtering

```javascript
sub vcl_backend_response
{
    // Mark all Set-Cookies for removal
    cookieplus.setcookie_keep("");

    // Only allow these Set-Cookies if the URL allows for it
    if (bereq.url ~ "^/admin") {
        cookieplus.setcookie_keep("JSESSIONID");
        cookieplus.setcookie_keep_regex("^BNES_SSESS");
        cookieplus.setcookie_keep_regex("^SSESS[a-zA-Z0-9]+$");
        cookieplus.setcookie_keep_regex("^SESS[a-zA-Z0-9]+$");
    }

    // Any Set-Cookie that is not kept will be removed
    cookieplus.setcookie_write();
}
```

You can find a comprehensive documentation of the cookieplus module in this [link](https://docs.varnish-software.com/varnish-enterprise/vmods/cookieplus/) from the official Varnish Enterprise documentation.

# HTTP Redirects

#### Redirect to HTTPS

When your website runs with SSL, it’s typical to want everything that comes in unencrypted via HTTP to be sent to the secure version to make sure nobody is browsing your website insecurely. To do so, you just have to call `redirect_https`.

```perl
 sub vcl_recv {  
     if (req.http.host == "www.transparentcdn.com") {
         call redirect_https;
     }
}
```

#### Redirects 301 and 302

Redirects can be performed by calling the function \``redirect_request`\`, see [callable functions](../../../config/vcl/callable-functions.md).

```perl
sub vcl_recv {
    # 301
    if (req.http.host == "example.com" && req.url ~ "^/old-section") {
        # req.http.tcdn-location header is required for this call to work.
        set req.http.tcdn-location = "301, https://www.example.com/new-section/";
        call redirect_request;
    }

    # 302
    if (req.http.host == "example.com" && req.url == "/") {
        set req.http.tcdn-location = "302, https://www.example.com/es/";
        call redirect_request;
    }
}
```

It’s important to understand the implications of the two types of redirects. A 301 is a redirect that, in theory, isn’t going to change, so it gets cached in browsers. If it does eventually change, it’s very complicated to un-cache that resource.

A 302, meanwhile, is a temporary redirect for a resource, so it’s not cached in browsers or intermediate proxies.

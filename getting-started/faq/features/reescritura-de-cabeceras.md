# Rewriting of headers

Because we are based on [Varnish](https://www.varnish-software.com/) Enterprise software, Transparent Edge offers great flexibility when it comes to changing your website’s default behavior.&#x20;

Rewriting headers is very common and rather easy to do. The most typical cases are:

#### Rewriting the Host header

The typical use case:

```javascript
sub vcl_recv{
  if ((req.http.host == "www.transparentcdn.com") && (bereq.url ~ "/blog")) {
    set http.host == "blog.transparentcdn.com"
    set req.backend_hint = c83_tcdn.backend();
  }
}
```

#### Rewriting the Cache-Control header

```javascript
sub vcl_backend_response {
     if ((bereq.http.host == "www.transparentcdn.com") && (bereq.url ~ "/my-new-url")) {
            set beresp.http.Cache-Control = "max-age=0, s-maxage=10";
            set beresp.ttl = 10s;
    }
}
```

#### Deleting headers

Based on the previous example, we can erase the Cache-Control header that comes from the origin before forcing the cache time.

```javascript
sub vcl_backend_response {    
    if ((bereq.http.host == "www.transparent.com") && (bereq.url ~ "/my-new-url")) {
            unset beresp.http.Cache-Control;
            set beresp.http.Cache-Control = "max-age=0, s-maxage=10";
            set beresp.ttl = 10s;
    }
}    
```

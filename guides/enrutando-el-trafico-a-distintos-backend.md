# Routing traffic to different backends

In Transparent CDN, it is possible to have a **"multi-origin"** configuration, where objects can be retrieved from different backends based on any element present in the request made by the browser to Transparent CDN.

A typical use case could be to switch origins based on the URL, so that, as shown in the example, everything that arrives at [https://www.transparentcdn.com/blog](https://www.transparentcdn.com/blog) goes to the backend c83\_tcdn\_blog, which has been previously [set up.](../getting-started/dashboard/auto-provisioning/backends.md)

```javascript
sub vcl_recv{
  if (req.http.host == "www.transparentcdn..com"){ 
    set req.backend_hint = c83_tcdn.backend();
  } 
  if ((req.http.host == "www.transparentcdn.com") && (bereq.url ~ "/blog")) {
    set req.backend_hint = c83_tcdn_blog.backend();
  }
}
```

The same logic presented here can be used to switch backends based on, as mentioned, any other element present in the request, such as a cookie or any other header.

# Blocking User-Agent

Blocking a User-Agent is straightforward. You just need to add these lines to your configuration file within the vcl\_recv function and customize it according to your needs.

```javascript
sub vcl_recv{
    if (req.http.User-Agent ~ "^curl") { 
        call deny_request;
    } 
}
```

In this example, any request with a User-Agent starting with the word curl will be denied with a 403 Forbidden response. The 403 response is triggered by calling `deny_request` which blocks the requests immediatel&#x79;**.**

---
description: >-
  Through the API, we provide our clients with a resource invalidation service.
  Below are the details and instructions for using it.
---

# Invalidation

The procedure for invalidating resources against the API is relatively simple. We will start by making a call to this URL:

```
https://api.transparentcdn.com/v1/companies/{COMPANY_ID}/invalidate/
```

In this request, we need to include a list or array within a JSON object where we will indicate the resources we want to invalidate, identified by their URLs.

```c
{
    "urls":[
            "http://www.servidor.com/recurso1",
            "http://fotos.servidor2.com/imagen2.jpg"
        ]
}
```

We can execute this call using [curl](https://www.mit.edu/afs.new/sipb/user/ssen/src/curl-7.11.1/docs/curl.html) or any interface that allows making HTTP requests. Here is an example of how a call would be composed to invalidate the resources _recurso1_ and _imagen2_.\


```json
curl -XPOST -H 'Authorization: Bearer TOKEN' -d '{"urls":["http://www.servidor.com/recurso1","http://fotos.servidor2.com/imagen2.jpg"]}' -H "Content-Type: application/json" https://api.transparentcdn.com/v1/companies/42/invalidate/
```

This request will return a JSON response, where we can find the **URLs** that have been sent for invalidation. The result would be as follows, assuming that "servidor.com" and "fotos.servidor.com" are registered as sites within our system and are associated with the client making the request.

```json
{
    "urls": [
        "http://fotos.servidor.com",
        "http://fotos.servidor.com/imagen2.jpg"
    ],
}
```

Our API provides a system for executing recursive invalidations. This means that all **URLs** that match the ones sent will be invalidated. To do this, an additional parameter needs to be included in the request: "ban" with a value of "1" to activate this service. Therefore, by executing the request with the following message, everything starting with _"/recursos/"_ from _fotos.servidor.com_ will be marked as invalid content.

```json
{
    "urls": [
        "http://fotos.servidor.com/recursos/"
    ],
    "ban": "1"
}
```


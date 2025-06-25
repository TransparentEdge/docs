# Making decisions based on HTTP headers

One of the advantages of using Varnish is its versatility, allowing us to make decisions based on any header that comes in the web request.

While we previously saw how to block content by [IP](../security/block-by-ip-address.md) or by [User-Agent](../security/block-by-ua.md) it is equally easy to make decisions of any kind based on the value or mere existence of an HTTP header.

### Changing the backend based on the header

Just as we explained [here](enrutando-el-trafico-a-distintos-backend.md) how to choose one backend or another based on a URL or host, it is equally simple to do it based on a header, for example, the [geo\_country\_code](../getting-started/faq/features/geolocalizacion-y-geobloqueo.md).

```javascript
sub vcl_recv{
    # Default backend
    set req.backend_hint = c82_tcdnes.backend();
    
    # Changing backend for Spanish users
    if (req.http.geo_country_code ~ "ES") { 
        set req.backend_hint = c82_tcdnes.backend();
    }
    
    # Changing backend for American users
    if (req.http.geo_country_code ~ "US") { 
        set req.backend_hint = c82_tcdnus.backend();
    }
    
}
```

### Allowing traffic only to users with a specific header

This is especially useful when you need to restrict access to your site for any reason but don't want to protect it with [Basic Authenticacion ](../getting-started/faq/cosas-a-tener-en-cuenta.md)because that would prevent the site from being cached.

```javascript
sub vcl_recv{
    if (req.http.auth-tcdn != "e37be3f5e06e263445654c0d6ba0e123") {
        call deny_request;
    }
}
```

This mechanism is very useful, but you need your browser to include that HTTP header in each request for your navigation to work. To do this, we recommend installing the [ModHeader](https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en) Chrome extension.


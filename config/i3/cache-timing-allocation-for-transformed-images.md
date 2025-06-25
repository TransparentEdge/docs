# Cache timing allocation for transformed images

In [i3](./), our image management solution, setting the `max-age` and `s-maxage` values in the [`Cache-Control`](../../getting-started/faq/glosario/cache-control.md) header is done through two custom headers: `TCDN-i3-max-age` and `TCDN-i3-s-maxage`, both with the same semantics.&#x20;

For example, if we wanted to serve images on our domain `mi-dominio.es` in [**WebP**](conversion-to-webp.md) format with a `max-age` of one minute (60 s) and an `s-maxage` of one month (2592000 s), we would simply need to deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard:](../../getting-started/dashboard/)

```c
i3 - Cache-Control
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-i3-transform = "auto_webp";
        set req.http.TCDN-i3-max-age = "60";
        set req.http.TCDN-i3-s-maxage = "2592000";
    }
}
```

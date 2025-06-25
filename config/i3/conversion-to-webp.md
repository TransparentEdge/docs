# Conversion to WebP

[i3](./), our image management solution, allows you to dynamically and transparently transform your images into the **WebP** format and serve them to browsers that explicitly declare compatibility with this format through the `Accept: image/WebP` header.&#x20;

To achieve this, we use our `TCDN-i3-transform` header, specifying the desired operation, which in this case is `auto_webp`.

For example, if you wanted to serve images on your domain `mi-dominio.es` in **WebP** format, you can simply deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard](../../getting-started/dashboard/):

```c
# i3 - auto_webp
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-i3-transform = "auto_webp";
    }
}
```

We also support an optional quality adjustment for the transformation to **WebP**, which is set to **80%** by default.&#x20;

The exact syntax for this operation is: `auto_webp:<porcentaje>%`.&#x20;

The following example illustrates this more clearly:

```c
# i3 - auto_webp - con ajuste de calidad
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-i3-transform = "auto_webp:90%";
    }
}
```

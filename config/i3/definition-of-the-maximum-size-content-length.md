# Definition of the maximum size (content-length)

[i3](./), our image management solution, allows you to dynamically and transparently restrict the maximum size (`Content-Length`) of your images by applying a progressive degradation in their quality.

To achieve this, we use our `TCDN-i3-transform` header, specifying the desired operation type, which in this case is `max_length.` Unlike other operations, such as [conversion to WebP format](conversion-to-webp.md), this operation requires a mandatory parameter: the maximum size that the resulting image will have. This parameter has the format `<size>[<unit>]`, where if no unit is specified, it is implicitly assumed to be in bytes. The accepted values for `<unit>` are `b` (bytes), `k` (kilobytes), and `m` (megabytes). For example, a valid value could be `640k.`

The exact syntax for this operation is as follows: `max_length:<size>[<unit>].`

Unlike the [quality](quality-adjustment.md) operation, this operation is not restricted to a specific image format.

For instance, if you wanted all the images in your domain `mi-dominio.es` under the URL `/estaticos/imagenes` to be served with a maximum size of 1 MiB, you would simply deploy a [VCL](../vcl/) configuration similar to the following from the [dashboard](../../getting-started/dashboard/):

```c
# i3 - max_length
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/") {
            set req.http.TCDN-i3-transform = "max_length:1m";
        }
    }
}
```

# Blurring

[i3,](./) our image management solution, allows you to dynamically and transparently blur your images.

To do this, we use our `TCDN-i3-transform` header, specifying the desired operation, which in this case is `blur.` Unlike other operations, such as the [conversion to WebP](conversion-to-webp.md) format, this operation requires an optional parameter for its execution: the percentage of blur to be applied to the original image.&#x20;

The exact syntax for this operation is as follows: `blur[:`_`<porcentaje>`_`]`. If no percentage is specified, the default value of **50%** will be assumed.&#x20;

For example, if you wanted all images on your domain `mi-dominio.es` that are located at the `URL /estaticos/imagenes` to be served with a 75% blur effect, you can simply deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard:](../../getting-started/dashboard/)

```c
# i3 - blur
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/") {
            set req.http.TCDN-i3-transform = "blur:75%";
        }
    }
}
```

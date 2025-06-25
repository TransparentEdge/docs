# Inclusion of graphics in the footer (strip)

[i3](./), our image management solution, allows you to dynamically and transparently include strips in your images.&#x20;

To do this, we use our `TCDN-i3-transform` header, specifying the desired operation, which in this case is `strip`. Unlike other operations, such as the [conversion to WebP format](conversion-to-webp.md), this operation requires a mandatory parameter for its execution: the URL of the strip that will be overlaid on the original image.&#x20;

The exact syntax for this operation is as follows: `strip:'`_`<url>`_`'`.&#x20;

For example, if you wanted all images on your domain `mi-dominio.es` that are located at the URL `/estaticos/imagenes` to be served with the strip available at `/estaticos/grafismos/faldon.png`, you can simply deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard](../../getting-started/dashboard/):

```c
# i3 - strip
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/") {
            set req.http.TCDN-i3-transform = "strip:'https://www.mi-dominio.es/estaticos/grafismos/faldon.png'";
        }
    }
}
```

Likewise, for the obtained result to be satisfactory, both images must have the same dimensions. Therefore, in certain situations, it will be necessary to combine this operation with the [automatic image resizing](automatic-resizing.md) operation itself.&#x20;

For example, taking the previous case, if the strip has dimensions of 800 x 600 pixels, in order for it to fit perfectly onto the image, we must resize it beforehand. Thus, you can simply deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard](../../getting-started/dashboard/):

```c
# i3 - strip
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/") {
            set req.http.TCDN-i3-transform = "resize:800x600, strip:'https://www.mi-dominio.es/estaticos/grafismos/faldon.png'";
        }
    }
}
```

Another possibility, for example, would be to have different strips available based on image _categories_, so that a specific strip is used depending on the category of the original image. To achieve this, you can simply deploy a configuration similar to the following:

```c
# i3 - strip
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/(ciudades|paisajes|patrones|texturas)/") {
            set req.http.category = regsub(req.url, "^/estaticos/imagenes/(ciudades|paisajes|patrones|texturas)/.*", "\1");
            set req.http.strip = "https://www.mi-dominio.es/estaticos/grafismos/faldones/" + req.http.category + ".png";
            set req.http.TCDN-i3-transform = "resize:800x600, strip:'" + req.http.strip + "'";
            unset req.http.category;
            unset req.http.strip;
        }
    }
}
```

Obviously, these are just small examples of very specific use cases. If you have any questions about how to integrate this functionality into your own domain, please don't hesitate to contact us at the email address [soporte@transparetncdn.com](mailto:soporte@transparetncdn.com).

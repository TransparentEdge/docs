# Automatic resizing

[i3,](./) our solution for image management, allows you to dynamically and transparently resize your images.&#x20;

To do this, we use our `TCDN-i3-transform` header, specifying the type of operation we want, in this case, `resize.` Unlike other operations, such as converting to WebP format, this operation requires a mandatory parameter: the dimensions of the resulting image. The parameter has the format `<width>x<height>`, with both units expressed in pixels and accepting a maximum value of 4096 pixels for each dimension. A valid value, for example, would be `400x300.`&#x20;

The exact syntax for this operation is as follows: `resize:`_`<ancho>`_`x[`_`<alto>`_`][,fixed]`.&#x20;

Alternatively, instead of expressing a dimension in pixels, you can use the variable `orig`, which refers to the original size of the image. A valid value, for example, would be `origx300.`&#x20;

Furthermore, the dimension `<height>`is optional: if it is not specified, it is automatically calculated based on the set `<width>` to maintain the aspect ratio. A valid value, for example, would be `400x.`&#x20;

Similarly, if we want to set dimensions and explicitly prevent the crop that is applied to maintain the aspect ratio, we can append the `fixed` suffix to the dimensions. A valid value, for example, would be `400x300,fixed.` Note that, for example, the value `400x,fixed` would also be valid, although the fixed suffix would be meaningless in this case.&#x20;

For example, if we wanted to serve images from our domain `mi-dominio.es` with a size of `300 x 300 pixels`, located at the URL `/estaticos/imagenes`, we would simply deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard](../../getting-started/dashboard/):

```c
# i3 - resize
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/") {
            set req.http.TCDN-i3-transform = "resize:300x300";
        }
    }
}
```

Another possibility, for example, would be if we wanted to serve the images from `/estaticos/imagenes` in a discrete set of different sizes (e.g., `320x240`, `640x480`, and `800x600`) according to the URL `/estaticos/imagenes/<width>x<height>.` To achieve this, we would simply need to deploy a configuration similar to the following:

```c
# i3 - resize
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/(320x200|640x400|800x600)/") {
            set req.http.size = regsub(req.url, "^/estaticos/imagenes/([0-9]+x[0-9]+)/(.*)", "\1");
            set req.url       = regsub(req.url, "^/estaticos/imagenes/([0-9]+x[0-9]+)/(.*)", "/estaticos/imagenes/\2");
            set req.http.TCDN-i3-transform = "resize:" + req.http.size;
            unset req.http.size;
        }
    }
}
```

Obviously, these are just small examples of very specific use cases. If you have any questions regarding how to integrate this functionality into your own domain, please don't hesitate to contact us at the email address [soporte@transparentedge.eu](mailto:soporte@transparetncdn.com).

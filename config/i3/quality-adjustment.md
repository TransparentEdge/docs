# Quality adjustment

i3, our image management solution, allows for dynamic and transparent adjustment of image quality.&#x20;

We achieve this by using our `TCDN-i3-transform` header and specifying the desired operation, in this case, `quality.` This operation requires a mandatory parameter: the quality level expressed as a percentage, indicating the resulting image's quality compared to the original image.&#x20;

The exact syntax for this operation is as follows: `quality:`_`<porcentaje>%`_&#x20;

However, there is an important restriction to note: this operation is only applicable to images in JPEG format ([conversion to WebP](conversion-to-webp.md) has its own quality adjustment). For images in other formats, you can alternatively consider using the [max\_length](definition-of-the-maximum-size-content-length.md) operation, which is not limited by the image format.&#x20;

For example, if we wanted all images on our domain `mi-dominio.es` under the URL `/estaticos/imagenes` to be served with a reduced quality of 75%, we would simply need to deploy a [VCL](../vcl/) [configuration](../../getting-started/dashboard/auto-provisioning/) similar to the following from the [dashboard](../../getting-started/dashboard/):

```c
# i3 - quality
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/estaticos/imagenes/") {
            if (urlplus.get_extension() ~ "(?i)(jpe?g)") {
                set req.http.TCDN-i3-transform = "quality:75%";
            }
        }
    }
}
```

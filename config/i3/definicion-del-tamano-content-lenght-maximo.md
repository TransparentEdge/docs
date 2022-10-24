# Definición del tamaño (content-lenght) máximo

[i3](./), nuestra solución para la [gestión imágenes](../../productos-y-servicios/i3-optimizacion-de-imagenes.md) permite restringir de manera dinámica y transparente  el tamaño (`Content-Length`) máximo tus imágenes, mediante un degradado progresivo en la calidad de las mismas.

Para ello, hacemos uso de nuestra cabecera `TCDN-i3-transform`, indicándole el tipo de operación que deseamos; en este caso, `max_length`. A diferencia de otras operaciones, como la [conversión a formato WebP](conversion-a-webp.md), esta operación requiere de un parámetro obligatorio para su ejecución: el tamaño máximo que tendrá la imagen resultante. Este parámetro tiene el formato _`<tamaño>[<unidad>]`_; así, si no se establece ninguna unidad, se asume implícitamente que el tamaño se expresa en _bytes_. Los valores admitidos para _`<unidad>`_ son `b` (_bytes_), `k` (_kilobytes_) y `m` (_megabytes_). Un valor válido, por ejemplo, sería `640k`.

La sintaxis exacta para esta operación es la siguiente: `max_length:`_`<tamaño>`_`[`_`<unidad>`_`]`.

A diferencia de la operación [`quality`](ajuste-de-la-calidad.md), ésta no está sometida a ninguna restricción con respecto al formato de la imagen.

Por ejemplo, si quisiéramos que todas las imágenes de nuestro dominio `mi-dominio.es` que _cuelgan_ de la URL `/estaticos/imagenes` se sirvieran ajustando el tamaño de las mismas a un máximo de 1 MiB, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

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

# Ajuste de la calidad

[i3](./), nuestra solución para la [gestión imágenes](../../productos-y-servicios/i3-optimizacion-de-imagenes.md) permite ajustar de manera dinámica y transparente la calidad tus imágenes.

Para ello, hacemos uso de nuestra cabecera `TCDN-i3-transform`, indicándole el tipo de operación que deseamos; en este caso, `quality`. A diferencia de otras operaciones, como la [conversión a formato WebP](conversion-a-webp.md), esta operación requiere de un parámetro obligatorio para su ejecución: el nivel de calidad, expresado éste en porcentaje, que tendrá la imagen resultante con respecto a la imagen original.

La sintaxis exacta para esta operación es la siguiente: `quality:`_`<porcentaje>`_.

Esta operación, sin embargo, tiene una restricción importante: tan sólo es aplicable a aquellas imágenes en formato JPEG. Para imágenes con otro formato, se puede considerar, alternativamente, el uso de la operación [`max_length`](definicion-del-tamano-content-lenght-maximo.md) ya que ésta no está sometida a ninguna restricción con respecto al formato de la imagen.

Por ejemplo, si quisiéramos que todas las imágenes de nuestro dominio `mi-dominio.es` que _cuelgan_ de la URL `/estaticos/imagenes` se sirvieran reduciendo su calidad a un 75%, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

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

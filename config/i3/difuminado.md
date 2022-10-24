# Difuminado

[i3](./), nuestra solución para la [gestión imágenes](../../productos-y-servicios/i3-optimizacion-de-imagenes.md) permite difuminar de manera dinámica y transparente tus imágenes.

Para ello, hacemos uso de nuestra cabecera `TCDN-i3-transform`, indicándole el tipo de operación que deseamos; en este caso, `blur`. A diferencia de otras operaciones, como la [conversión a formato WebP](conversion-a-webp.md), esta operación requiere de un parámetro opcional para su ejecución: el porcentaje de difuminado que se aplicará sobre la imagen original.

La sintaxis exacta para esta operación es la siguiente: `blur[:`_`<porcentaje>`_`]`. En caso de que no se indique ningún porcentaje, se asumirá `50%` como valor por defecto.

Por ejemplo, si quisiéramos que todas las imágenes de nuestro dominio `mi-dominio.es` que _cuelgan_ de la URL `/estaticos/imagenes` se sirvieran siendo difuminadas en un 75%, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

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

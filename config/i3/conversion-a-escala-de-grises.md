# Conversión a escala de grises

[i3](./), nuestra solución para la [gestión imágenes](../../productos-y-servicios/i3-optimizacion-de-imagenes.md) permite convertir de manera dinámica y transparente tus imágenes a escala de grises.

Para ello, hacemos uso de nuestra cabecera `TCDN-i3-transform`, indicándole el tipo de operación que deseamos; en este caso, `grayscale`.

Por ejemplo, si quisiéramos servir en escala de grises las imágenes de nuestro dominio `mi-dominio.es`, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

```c
# i3 - grayscale
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-i3-transform = "grayscale";
    }
}

```

# Asignación de los tiempos de caché en las imágenes transformadas

En [i3](./), nuestra solución para la [gestión imágenes](../../productos-y-servicios/i3-optimizacion-de-imagenes.md), la asignación de los valores `max-age` y `s-maxage` en la cabecera [`Cache-Control`](../../getting-started/faq/glosario/cache-control.md) se lleva a cabo por medio de dos cabeceras propias, `TCDN-i3-max-age` y `TCDN-i3-s-maxage`, con idéntica semántica.

Por ejemplo, si quisiéramos servir en formato [**WebP**](conversion-a-webp.md) las imágenes de nuestro dominio `mi-dominio.es`, y que éstas tuvieran un `max-age` de un minuto (60 s) y un `s-maxage` de un mes (2592000 s), nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

```c
# i3 - Cache-Control
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-i3-transform = "auto_webp";
        set req.http.TCDN-i3-max-age = "60";
        set req.http.TCDN-i3-s-maxage = "2592000";
    }
}
```

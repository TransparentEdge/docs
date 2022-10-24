# Inclusión de grafismo en faldón (strip)

[i3](./), nuestra solución para la [gestión imágenes](broken-reference) permite incluir de manera dinámica y transparente faldones (_strips_) en tus imágenes.

Para ello, hacemos uso de nuestra cabecera `TCDN-i3-transform`, indicándole el tipo de operación que deseamos; en este caso, `strip`. A diferencia de otras operaciones, como la [conversión a formato WebP](conversion-a-webp.md), esta operación requiere de un parámetro obligatorio para su ejecución: la URL del faldón (_strip_) que se superpondrá sobre la imagen original.

La sintaxis exacta para esta operación es la siguiente: `strip:'`_`<url>`_`'`.

Por ejemplo, si quisiéramos que todas las imágenes de nuestro dominio `mi-dominio.es` que _cuelgan_ de la URL `/estaticos/imagenes` se sirvieran con el faldón (_strip_) disponible en `/estaticos/grafismos/faldon.png`, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

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

&#x20;Asimismo y para que el resultado obtenido sea satisfactorio, ambas imágenes han de tener las mismas dimensiones; por ello, en determinadas situaciones será necesario combinar esta operación con la propia para el [redimensionamiento automático](redimensionamiento-automatico.md) de imágenes.

Por ejemplo, retomando el caso anterior, si el faldón (_strip_) tuviera unas dimensiones de 800 x 600 píxeles, para que éste se acomode perfectamente sobre la imagen, previamente deberemos redimensionarla. Así,  nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

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

Otra posibilidad, por ejemplo, sería que dispusiéramos de distintos faldones (_strips_) en función de _categorías_ de imágenes, de modo que se emplease uno u otro de acuerdo con la categoría de la imagen original. Para ello, nos bastaría con desplegar una configuración similar a la siguiente:

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

Obviamente, éstos no son más que pequeños ejemplos de casos de uso muy específicos. Si tienes cualquier duda respecto a cómo integrar esta funcionalidad en tu propio dominio, por favor, no dudes en contactarnos a través de la dirección de correo electrónico [soporte@transparentedge.eu](mailto:soporte@transparetncdn.com).

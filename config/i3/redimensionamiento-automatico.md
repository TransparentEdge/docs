# Redimensionamiento automático

[i3](./), nuestra solución para la [gestión imágenes](broken-reference) permite redimensionar de manera dinámica y transparente tus imágenes.

Para ello, hacemos uso de nuestra cabecera `TCDN-i3-transform`, indicándole el tipo de operación que deseamos; en este caso, `resize`. A diferencia de otras operaciones, como la [conversión a formato WebP](conversion-a-webp.md), esta operación requiere de un parámetro obligatorio para su ejecución: las dimensiones que tendrá imagen resultante. Este parámetro tiene el formato _`<ancho>`_`x`_`<alto>`_, expresadas ambas unidades en píxeles y admitiendo un valor máximo, para cada dimensión, de 4096 píxeles. Un valor válido, por ejemplo, sería `400x300`.

La sintaxis exacta para esta operación es la siguiente: `resize:`_`<ancho>`_`x[`_`<alto>`_`][,fixed]`.

Alternativamente, en lugar de expresar una dimensión en píxeles, se puede emplear la variable `orig`, que hace referencia al tamaño original de la imagen. Un valor válido, por ejemplo, sería `origx300`.

Por otra parte, la dimensión _`<alto>`_ es opcional: si ésta no se indica, se calcula automáticamente de acuerdo con el _`<ancho>`_ fijado para mantener la relación de aspecto (_aspect ratio_). Un valor válido, por ejemplo, sería `400x`.

Asimismo, si deseamos establecer unas dimensiones e impedir explícitamente que se aplique el recorte (_crop_) que se realiza para mantener la relación de aspecto, podemos aplicar el sufijo `fixed` a las dimensiones.  Un valor válido, por ejemplo, sería `400x300,fixed`. Nótese igualmente que, por ejemplo, el valor `400x,fixed` sería igualmente válido aunque aquí el sufijo `fixed` carecería de sentido.

Por ejemplo, si quisiéramos servir con un tamaño de 300 x 300 píxeles las imágenes de nuestro dominio `mi-dominio.es` que _cuelgan_ de la URL `/estaticos/imagenes`, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../vcl/) similar a la siguiente:

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

Otra posibilidad, por ejemplo, sería que deseáramos servir las imágenes de `/estaticos/imagenes` en un conjunto discreto de distintos tamaños (por ejemplo: `320x240`, `640x480` y `800x600`) de acuerdo con la URL `/estaticos/imagenes/`_`<ancho>`_`x`_`<alto>`_. Para ello, nos bastaría con desplegar una configuración similar a la siguiente:

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

Obviamente, éstos no son más que pequeños ejemplos de casos de uso muy específicos. Si tienes cualquier duda respecto a cómo integrar esta funcionalidad en tu propio dominio, por favor, no dudes en contactarnos a través de la dirección de correo electrónico [soporte@transparentedge.eu](mailto:soporte@transparetncdn.com).


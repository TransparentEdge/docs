# Contenido protegido por token

Esta función nos permite proteger contenido de nuestro sitio _web_ mediante la definición de _tokens_ cuyo periodo de validez puede ser controlado de manera programática.

### Explicación teórica&#x20;

Con este propósito en mente, la idea es que cualquier solicitud de un objeto (_asset_) concreto de nuestro sitio _web_, por ejemplo, un manifiesto DASH (_Dynamic Adaptive Streaming over HTTP_) o HLS (_HTTP Live Streaming_), se realice indicando, necesariamente, el valor de tres parámetros obligatorios:

#### Parámetros necesarios

1. El instante a partir del cual comienza la validez del _token_ (`vf`, _valid from_).
2. El instante en el cual expira la validez del _token_ (`vu`, _valid until_).
3. El _hash_ MD5 correspondiente con dicho _token_ (`h`). Este _hash_ viene determinado, a su vez, por el siguiente patrón: _`<vf>`_`@`_`<vu>`_`@`_`<secret>`_`@`_`<url excluyendo los parámetros obligatorios relativos al token (vf, vu, h)>`_.

Por ejemplo, si en nuestro sitio _web_ `mi-dominio.es` queremos proteger con un _token_ el recurso `/lista-reproduccion.m3u8?lang=es`, la petición se deberá formular incluyendo, además, los parámetros obligatorios anteriormente anunciados: `/lista-reproduccion.m3u8?lang=es&vf=`_`<vf>`_`&vu=`_`<vu>`_`&h=`_`<h>`_.

Asimismo, el valor de este parámetro `h` será el _hash_ MD5 correspondiente con la cadena _`<vf>`_`@`_`<vu>`_`@`_`<secret>`_`@/lista-reproduccion.m3u8?lang=es`; y, de igual manera, _`<secret>`_ será la semilla (_seed_) empleada en la generación del _token_.

### ¿Cómo se utiliza?

Esta función se invoca a través de nuestra cabecera `TCDN-Command`; así, debemos incluir el valor `protect-with-token` junto con el conjunto de parámetros, separados por el carácter _dos puntos_ (`:`), necesarios por ésta. Concretamente, la sintaxis de la función es la siguiente: protect`-with-token:secret=`_`<secret>`_`[:vf=`_`<valid from>`_`][:vu=`_`<valid until>`_`][:h=`_`<hash>`_`]`.

_`<secret>`_ es el único de estos parámetros que es obligatorio y define la semilla (_seed_) que se empleará en la generación del _token_; es decir, se trata de la clave privada sobre la que se vertebra todo el proceso.

Opcionalmente, se pueden indicar los parámetros _`<valid from>`_, _`<valid until>`_ y _`<hash>`_ de modo que éstos quedan codificados manera estricta (_hard-coded_) en la [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) correspondiente. Sin embargo, existe la posibilidad de determinar dinámicamente el valor para estos tres parámetros: bien en la propia petición (_request_) a través de su _query string_ o bien mediante la asignación de _cookies_ homónimas (`vf`, `vu` y `h`).

#### Ejemplo de _token_ con periodo de validez estático

Por ejemplo, si en nuestro dominio `mi-dominio.es` quisiéramos proteger con un _token_ el recurso referido anteriormente, `/lista-reproduccion.m3u8?lang=es`, estableciendo un periodo de validez del mismo por un año (por ejemplo, 2022), nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# protect-with-token
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/lista-reproduccion.m3u8\?lang=es") {
            set req.http.TCDN-Command = "protect-with-token:secret=ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM:vf=1640991600:vu=1672527599"; # desde '01/01/2022 00:00:00' hasta '12/31/2022 23:59:59'.
        }
    }
}
```

En el ejemplo anterior vemos cómo se ha definido la cadena `ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM` como valor para el parámetro obligatorio _`<secret>`_. Además, se han proporcionado los valores para dos de los otros tres parámetros restantes, _`<valid until>`_ y _`<valid from>`_; asignándoles, respectivamente, los valores `1640991600` (1 de enero de 2022) y `1672527599` (31 de diciembre de 2022).

```bash
$ date --date='01/01/2022 00:00:00' +'%s'
1640991600
$ date --date='12/31/2022 23:59:59' +'%s'
1672527599
$
```

En consecuencia, el único parámetro restante es `h`, que se corresponde con el _hash_ MD5 relativo a la cadena `1640991600@1672527599@ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM@/lista-reproduccion.m3u8?lang=es`, es decir, `3caf5c965d2895f1705481d3a32d63b4`.

```
$ echo -n "1640991600@1672527599@ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM@/lista-reproduccion.m3u8?lang=es" | md5sum | awk '{ print $1 }'
3caf5c965d2895f1705481d3a32d63b4
$
```

Así, las peticiones, al recurso `/lista-reproduccion.m3u8?lang=es` que se formulen sin el parámetro `h` o con un parámetro `h` no válido devolverán un _status code_ 401 (_Unauthorized_); si se realizan antes del inicio del periodo de validez, devolverán un _status code_ 404 (_Not yet valid token_); o tras el fin de dicho periodo de validez, un 410 (_Expired token_).

Tan sólo las peticiones `/lista-reproduccion.m3u8?lang=es&h=3caf5c965d2895f1705481d3a32d63b4` o, en su defecto, aquellas en donde se haya definido una _cookie_ `h` con el valor correcto, devolverán el recurso (_asset_) solicitado en conjunción con un _status code_ 200.

#### Ejemplo de _token_ con periodo de validez dinámico

En el ejemplo anterior, el periodo de validez del _token_ se encuentra codificado de manera estricta en la propia configuración; si deseamos que éste sea dinámico, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# protect-with-token
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/lista-reproduccion.m3u8\?lang=es") {
            set req.http.TCDN-Command = "protect-with-token:secret=ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM";
        }
    }
}
```

De este modo, los parámetros `vf` y `vu` deberán ser, en este caso, proporcionados, bien a través de la _query string_ en la petición (_request_) o bien mediante sendas _cookies_. Asimismo, puesto que `vf` y `vu` ahora son parámetros dinámicos, el valor de `h` también lo será, al ser dependiente de éstos.

Obviamente, éstos no son más que pequeños ejemplos de un caso de uso muy específico. Si tienes cualquier duda respecto a cómo integrar esta funcionalidad en tu propio dominio, por favor, no dudes en contactarnos a través de la dirección de correo electrónico [soporte@transparentcdn.com](mailto:soporte@transparetncdn.com).

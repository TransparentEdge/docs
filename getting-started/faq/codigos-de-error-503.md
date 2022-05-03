# Códigos de error 503

## Introducción

Cuando un [servidor web](https://es.wikipedia.org/wiki/Servidor\_web) no consigue procesar una petición o directamente no está disponible, nosotros al otro lado en nuestro navegador web recibimos un código de respuesta de error **`5xx`**, es decir, un código entre `500` y `599`.

Estos son **códigos de error del servidor web**, que son muy distintos a otros como el `404` - _Not found_, que se produce normalmente cuando pedimos un objeto que no existe en el servidor.

Algunos de los códigos de error más frecuentes serían:

* **500** - _Internal Server Error_
* **502** - _Bad Gateway_
* **503** - _Service Unavailable_
* **504** - _Gateway Timeout_

Las CDN's, por motivos técnicos, enmascaramos esa almalgama de _status codes_ en uno sólo. Nosotros en Transparent Edge Services **utilizamos el código `503` para cualquier error de servidor `5xx`.**

Ya que un error **`503`** proveniente de la CDN puede tener muchos motivos, éstos vienen clasificados en las cabeceras de respuesta y en el própio código html del error devuelto con más información relevante que puede ayudar a discernir el tipo de error, dichas cabeceras empiezan por `TCDN-` y todas tienen al final un valor numérico que nos permite identificar con precisión la petición, por ejemplo: `TCDN-BENG-504:275330054`.

## Lista de códigos

* **`TCDN-BENG-5xx`** - _Backend Error No Grace_

> No tenemos objeto en caché y origen ha respondido con un error **`5xx`** no catalogado en las etiquetas inferiores, el número que se muestra en la cabecera es exactamente el código que devolvió origen, por ejemplo: `TCDN-BENG-504`.

* **`TCDN-SBPR`** - _Sick Backend POST/PUT Request_

> La petición no es cacheable y origen ha dado error.

* **`TCDN-SBNG`** - _Sick Backend No Grace_

> Origen está marcado como caído (su _healthcheck_ está fallando) y no tenemos el objeto en caché.

* **`TCDN-DNF`** - _Domain Not Found_

> El dominio no ha sido encontrado, no está registrado en la CDN.

* **`TCDN-NBD`** - _No Backend Defined_

> El dominio existe en la CDN pero no se le ha asignado ningún backend.

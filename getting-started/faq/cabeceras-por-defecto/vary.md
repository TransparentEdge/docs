# Vary

La cabecera Vary es una cabecera útil a la vez que peligrosa.\
A grandes rasgos, la cabecera Vary se usa para decir a las cachés (ya sea el del navegador, un _proxy_ o una CDN como Transparent Edge Services) que guarde versiones del mismo objeto en función de una cabecera dada. Pongamos algunos ejemplos de buen uso y mal uso de esta cabecera:[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Cabecera\_Vary/edit?section=2)

#### Mal uso de la cabecera Vary:

Un mal uso de la cabera Vary sería, por ejemplo, el siguiente:

```
HTTP/1.1 200 OK
Vary: Cookies
Cache-control: max-age=86400
Content-Type: text/html; charset=UTF-8
TSSecure: SecureLayer
client-id: 4
TP-l2-Cache: MISS
X-Device: desktop
Date: Mon, 19 Oct 2015 15:24:05 GMT
Age: 85783
Connection: keep-alive
TP-Cache: HIT
```

En las cabeceras de respuesta de la petición de arriba, vemos que se esta enviando la cabecera "Vary: Cookies". Esto lo que hace es instar a las cachés a guardar una versión del mismo objeto (por ejemplo, una imagen o una portada) por cada _cookie_ distinta que llegue. Es decir, este tipo de petición tendría un _hit ratio_ muy bajo, posiblemente por debajo del 10 %, por lo que el sistema de cachés no sería efectivo. Otro ejemplo de mal uso sería el "Vary: User-Agent", que nos guardaría una versión del mismo objeto por cada User-Agent que nos visite y, os avisamos: son unos cuantos los User-Agent que hay.

Un buen uso de esta cabecera lo encontramos en el siguiente ejemplo:\


```
HTTP/1.1 200 OK
Vary: Accept-Encoding
Cache-control: max-age=86400
Content-Type: text/html; charset=UTF-8
TSSecure: SecureLayer
client-id: 4
TP-l2-Cache: MISS
X-Device: desktop
Date: Mon, 19 Oct 2015 15:24:05 GMT
Age: 85783
Connection: keep-alive
TP-Cache: HIT
```

Aquí guardamos una versión del mismo objeto en función del si el objeto está comprimido o no. Esto se hace para guardar retrocompatibilidad con navegadores antiguos y es el típico caso de buen uso de esta cabecera.\


Otro ejemplo del buen uso seria la cabecera "Vary: X-Device". Con el Vary de está manera le decimos a Transparent CDN que guarde una misma versión del objeto en función de la cabecera X-Device, que puede tener el valor de _desktop_, _mobile_ o _tablet_.&#x20;

Puedes ver más sobre la detección de móviles [aquí](../funcionalidades/deteccion-de-dispositivos.md):

Más sobre esta cabecera [aquí](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.htm).

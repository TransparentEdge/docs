# Vary

La cabecera _Vary_ es una cabecera útil a la vez que peligrosa. Se usa para decir a las cachés (ya sea la del navegador, un proxy o una CDN como Transparent Edge) que guarden versiones del mismo objeto en función de una cabecera dada. Veamos algunos ejemplos de buen y mal uso de ella:

### Mal uso de la cabecera Vary

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

En este ejemplo, en las cabeceras de respuesta de la petición de arriba, vemos que se está enviando la cabecera _Vary: Cookies_. Esta acción insta a las cachés a guardar una versión del mismo objeto (por ejemplo, una imagen o una portada) por cada _cookie_ distinta que llegue. Es decir, este tipo de petición tendría un _hit ratio_ muy bajo, posiblemente por debajo del 10 %, por lo que el sistema de cachés no sería efectivo.&#x20;

Otro ejemplo de mal uso sería el _Vary: User-Agent_, que nos guardaría una versión del mismo objeto por cada _user-agent_ que nos visite. Y, te avisamos, los _user-agent_ que hay son unos cuantos.

### Buen uso de la cabecera _Vary_

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

Aquí guardamos una versión del mismo objeto en función del si el objeto está comprimido o no. Esto se hace para guardar retrocompatibilidad con navegadores antiguos y es el típico caso de buen uso de esta cabecera.

Otro ejemplo de buen uso sería la cabecera _Vary: X-Device_. Con el _Vary_ de esta manera, decimos a Transparent Edge que guarde una misma versión del objeto en función de la cabecera _X-Device_, que puede tener el valor de _desktop_, _mobile_ o _tablet_.

Puedes ver más sobre la detección de móviles [aquí](../funcionalidades/deteccion-de-dispositivos.md).

Más sobre esta cabecera [aquí](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.htm).

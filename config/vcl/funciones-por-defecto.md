# Funciones por defecto

## [Del lado del cliente](https://varnish-cache.org/docs/trunk/users-guide/vcl-built-in-subs.html)

### vcl\_recv

Esta función se llama cuando entra una _request_ nueva. Desde esta función puedes modificar el comportamiento o contenido de la _request_ y decidir cómo procesarla.&#x20;

En esta función se debe configurar un [_backend_](../../getting-started/dashboard/autoprovisionamiento/sites-and-backends.md) __ al que enviar la petición.



vcl\_(recv|miss|backend\_fetch|backend\_response|hash|deliver)

### vlc\_hash

Llamada después de la vcl\_recv. En esta función se modifica el comportamiento a la hora de almacenar el objeto en la caché.

### vcl\_miss

Se llama inmediatamente después de buscar el objeto en la caché y siempre y cuando este no haya sido encontrado.

### vlc\_deliver

Esta función se llama antes de enviar el objeto al cliente.

## Del lado del origen

### vcl\_backend\_fetch

Esta función se llama antes de enviar la _request_ al _backend_ para recuperar el objeto.

### vcl\_backend\_response

Está función es llamada cuando llega la respuesta del _backend_ de forma exitosa.



{% hint style="warning" %}
En el resto de funciones predefinidas en Varnish Enterprise plus no está permitida la modificación por parte del usuario en Transparent Edge Services para garantizar en todo momento la seguridad y estabilidad de los _sites_ de nuestros clientes. No obstante, [aquí](https://varnish-cache.org/docs/trunk/users-guide/vcl-built-in-subs.html) puedes consultarlas.
{% endhint %}

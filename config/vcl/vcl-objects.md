# VCL Objects

En VCL hay diferentes tipos de objetos que tienes que conocer. A estos objetos se puede acceder desde la configuración VCL y así pueden ser modificados.

* **req.** Es el objeto de la _request_. Se accede principalmente desde la función vcl\_recv. Cuando Varnish recibe la _request_, este objeto es creado y, desde ese momento, es accesible.
* **bereq.** Es el objeto que se envía al _backend_. Se crea justo antes de enviar el objeto al _backend_ u origen. Está basado en el objeto req.
* **beresp.** Es la respuesta del _backend_. Contiene las cabeceras de la respuesta del _backend_ a Transparent CDN. Si quieres modificarlo, este objeto es accesible desde la función **vcl\_backend\_response.**
* **resp.** Es la respuesta HTTP justo antes de ser enviada al cliente. Puedes modificar este objeto en la función **vcl\_deliver.**
* **obj.** Es un objeto de solo lectura y es el objeto que está almacenado en la caché.


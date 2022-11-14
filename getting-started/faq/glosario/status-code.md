# Status Code

Un _status code_ es un código de respuesta HTTP o HTTPS que utiliza el servidor para hacer saber al cliente el estado de la petición y si esta tuvo éxito o no.

En la [RFC 2616, sección 10](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) podemos ver todos los _status code_ que soporta HTTP 1.1, pero aquí vamos a citar los mas relevantes.

## Respuesta exitosa

**200 OK**

La petición ha sido exitosa y el servidor devuelve un _status code_ 200 para indicarselo al cliente.[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Status\_code/edit?section=4)

**206 **_**Partial Content**_

El contenido devuelto no es todo el contenido. Este tipo de respuestas se da ante peticiones de tipo _range_, cuando el cliente pide explícitamente una parte del objeto mediante la cabecera Content-Range.

## Redirecciones

**301 Moved Permanently**

Este _status code_ indica una redirección permanente, es decir, no se ha de volver a cambiar. Este tipo de peticiones son cacheables tanto por proxy cache como por navegadores. Hay que tener cuidado al usarlas porque pueden confundir.

**302 **_**Found**_

Se usa para identificar una redirección temporal, por lo que no se cachea nunca en _proxys_ ni en navegadores.

**304 **_**Not Modified**_

Nos indica que el contenido no ha cambiado en el servidor. Para saberlo, tanto los navegadores como los _proxies_ envían a los servidores web de los clientes la petición junto con la cabecera If-Modified-Since.

## Errores de cliente

**400 **_**Bad Request**_

Vemos este tipo de error cuando el servidor no entiende la petición que le está mandando el cliente. Por lo general, no se suelen ver en los navegadores y se ven más cuando son procesos o aplicaciones que construyen su propia petición HTTP. Suele deberse a una petición mal formada.

**401 **_**Unauthorized**_

En ocasiones se necesita que un _site_ o parte de él este protegido bajo usuario/contraseña y es aquí cuando este _status code_ nos indica un acceso no autorizado al recurso. Las peticiones que llevan autenticación no se cachean en Transparent Edge.

**403 **_**Forbbiden**_

Nos indica que estamos entrando a una parte restringida del _site_ al cual se nos deniega el acceso, posiblemente por motivos de seguridad. Puede ser por una restricción de IP o porque está intentando explotar alguna vulnerabilidad de la aplicación web. En Transparent Edge, esto último ocurre solo si se cuenta con la capa de seguridad Secure Layer.

**404 **_**Not Found**_

Este tipo de error es muy común. Significa simplemente que el recurso web que estamos solicitando no se encuentra en el servidor.

**405 Method not Allowed**

Este código de estado es devuelto cuando el cliente hace una petición HTTP mediante un protocolo no soportado por la caché o servidor. En Transparent Edge están permitidos los siguientes metodos:

```
GET
POST
PUT
DELETE
OPTIONS
PROPFIND
PUSH
HEAD
```

**408 **_**Timeout**_

Es un error complejo. Significa que la conexión se ha cerrado por superación del tiempo de espera. Si el error se produce en el servidor del cliente, ese error se transformará en un 503 en Transparent Edge. En condiciones normales suele deberse a algún problema de comunicaciones, ya sea por algún _firewall_ interno, un error interno del servidor o un _firewall_/_proxy_ entre Transparent Edge y el cliente. Cualquier mensaje de "c_onnection close_" o "_closing connection_" en los _logs_ del servidor de cliente puede significar el 408. Hemos tenido problemas históricos con módulos de Apache que provocaban ese error, como el mpm\_itk ([http://mpm-itk.sesse.net/](http://mpm-itk.sesse.net/)).

## Errores de servidor

**500 Internal Server Error**

Error de servidor, por lo general de aplicación. Por ejemplo, pueden provocarlo unas comillas mal cerradas en PHP.

**502 **_**Bad Gateway**_

Es un código devuelto por algunos _web server_ cuando actúan como _proxy_ inverso y el recurso que está detrás no está disponible. Muy típico de Nginx.

**503 **_**Service Unavailable**_

El servicio no está disponible. Este error es devuelto cuando no se puede contactar con el servidor web del cliente, bien porque está caído, bien por un problema de comunicaciones. Este error es generado "al vuelo" por Transparent Edge cuando hay algún error fatal en el cliente. Cuando se reciben los 503 en el cliente, hay que buscar por cualquier tipo de error en sus _logs._

**504 **_**Gateway Timeout**_

Este código de respuesta suele darse cuando el servidor de origen del cliente esta tardando mucho en devolver un objeto no cacheado, bien porque no debe cachearse o bien porque ha caducado.

Transparent Edge te protege ante todos los errores 5XX mostrando la última versión cacheada. Puedes ver más información [aquí](../funcionalidades/proteccion-ante-caidas-del-origen.md).

# Protección ante caídas del origen

Ante problemas de conectividad entre los servidores de Transparent Edge y los servidores de origen del cliente, ya sea por caída del servicio o por cualquier otro problema que impida recuperar el contenido del origen, Transparent Edge servirá una versión caducada del contenido.&#x20;

Este comportamiento se mantendrá hasta un máximo de ocho horas o hasta el restablecimiento de las comunicaciones entre Transparent Edge y los servidores de origen del cliente.

Para que Transparent Edge pueda servir ese contenido caducado en caso de problemas, es necesario que los objetos servidos tengan un tiempo de caché (_Cache-Control: **max-age**_) superior a 1 segundo. Es decir, los objetos marcados con cabeceras no cacheables (_No-Store, No-Cache, Private_, etc.) no serán servidos en caso de problemas, ya que no existen en la caché, por lo que estas peticiones devolverán un error HTTP 503.\

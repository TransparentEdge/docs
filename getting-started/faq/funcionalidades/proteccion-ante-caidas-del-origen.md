# Protección ante caídas del origen

Ante problemas de conectividad entre los servidores de Transparent Edge Services y los servidores de origen del cliente, ya sea por caída del servicio o cualquier otro problema que impida recuperar el contenido del origen, Transparent Edge Services servirá una versión caducada del contenido.\
Este comportamiento se mantendrá hasta un máximo de ocho horas o hasta el restablecimiento de las comunicaciones entre  Transparent Edge Services y los servidores de origen del cliente.

Para que Transparent Edge Services pueda servir ese contenido caducado en caso de problemas, es necesario que los objetos servidos tengan un tiempo de caché (cache-control: max-age) superior a 1 segundo. Es decir, los objetos marcados con cabeceras no cacheables (no-store, no-cache, private, etc.) no serán servidos en caso de problemas, ya que no existen en la caché, por lo que estas peticiones devolverán un error HTTP 503.

# HTTP/2

HTTP/2 es una actualización importante del protocolo HTTP que hace un uso más eficiente de los recursos de la red. Compatible con HTTP/1.1., no implica cambios significativos en conceptos básicos como los métodos HTTP, los campos de cabecera y los códigos de estado.

Sin embargo, sí agrega innumerables mejoras, como el uso de una única conexión, la compresión de cabeceras o el servicio _server push_ o _cache push_ que, mediante estimaciones, habilita al servidor a enviar información al usuario antes de que este la requiera. Así se logra, por ejemplo, que al solicitar una página HTML que enlaza hojas de estilo, imágenes u otros recursos, se envíe todo conjuntamente en la primera solicitud (envío multiplexado) y no tras la recepción del fichero HTML.

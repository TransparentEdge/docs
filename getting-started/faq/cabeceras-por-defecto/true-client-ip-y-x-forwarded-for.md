# True-Client-IP y X-Forwarded-For

La dirección IP del cliente queda enmascarada por la red de cachés de Transparent Edge. Esto quiere decir que el servidor de origen del cliente siempre va a recibir conexiones de una dirección IP de **nuestra red de cachés**. Además, dicha red de cachés varía en el tiempo, con lo cual no se puede filtrar a nivel de dirección IP (nivel 3 OSI).

Para ello, lo que hacemos en Transparent Edge es crear cabeceras HTTP extra al estándar que nos permiten pasar al cliente la dirección IP del usuario real que está navegando. De esta forma, el cliente puede tomar las medidas oportunas con la dirección aunque el servidor esté detrás de la red de caché. Eso se realiza con las cabeceras _X-Forwarded-For_ y/o _True-Client-IP_.

Ahora le toca al cliente poder capturar dicha cabecera y operar con ella.

### ¿Qué diferencia hay entre X-Forwarded-For y True-Client-IP?

_**X-Forwarded-For**_ es una cabecera HTTP estándar donde los diferentes _softwares_ de _proxy_ añaden la dirección del cliente, incluso los sistemas corporativos. Esto quiere decir que si las peticiones atraviesan varias capas, varias direcciones aparecerán separadas por comas en dicha cabecera. Eso implica que el cliente ha de "separar" la última dirección IP o analizar varias de ellas.

_**True-Client-IP**_ es la última dirección IP antes de llegar a los servidores de Transparent Edge. Correspondería a la primera IP de _X-Forwarded-For_. Por lo tanto, corresponde con la dirección real del cliente que inició la conexión.

###

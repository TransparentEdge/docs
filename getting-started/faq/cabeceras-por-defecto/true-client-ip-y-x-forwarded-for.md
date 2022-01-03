# True-Client-Ip (y x-forwarded-for)

La dirección IP del cliente queda enmascarada por la red de cachés de Transparent Edge Services. Esto quiere decir que el servidor de origen del cliente siempre va a recibir conexiones de una dirección IP de **nuestra red de cachés**. Además, dicha red de cachés varía en el tiempo, con lo cual no se puede filtrar a nivel de dirección IP (nivel 3 OSI).

Para ello, lo que hacemos en Transparent Edge Services es crear cabeceras HTTP extra al estándar que nos permiten pasar al cliente la dirección IP del usuario real que está navegando. De esta forma, el cliente puede tomar las medidas oportunas con la dirección aunque el servidor esté detrás de la red de caché. Eso se realiza con las cabeceras **x-forwarded-for** y/o **true-client-ip.**

Ahora le toca al cliente poder capturar dicha cabecera y operar con ella.

### &#x20;¿Qué diferencia hay entre x-forwarded-for y TRUE-CLIENT-IP?

**X-forwarded-for** es una cabecera HTTP estándar donde los diferentes softwares de _proxy_ añaden la dirección del cliente, incluso los sistemas corporativos. Esto quiere decir que si las peticiones atraviesan varias capas , varias direcciones aparecerán separadas por comas en dicha cabecera. Eso implica que el cliente ha de "separar" la última dirección IP o analizar varias de ellas.

**True-client-ip** es la última dirección IP antes de llegar a los servidores de Transparent Edge Services. Correspondería a la primera IP de X-Forwarded-For. Por lo tanto, corresponde con la dirección real del cliente que inició la conexión.

###

# Envío de logs

En este punto tenemos dos opciones incluidas dentro de cualquier paquete de servicio para hacer llegar los logs al cliente.&#x20;

Por un lado tenemos una opción básica mediante la cual ponemos a disposición del cliente sus  logs en un FTP/SFTP propiedad del mismo. En esta opción los ficheros se envían cada hora un fichero comprimido por cada una de las máquinas que este dando servicio al cliente.

El nombre del fichero tiene es siguiente formato:

```shell
2021072913-XX-france-b3fc576dcd50e32869c5873349ca5e7c.log.gz
```

Donde los primeros 10 dígitos corresponden con la fecha del envío.&#x20;

El siguiente campo entre guiones medios corresponde con el ID de cliente.

Lo siguiente es el país donde está la máquina que sirve las peticiones.

Por último un md5 interno que identifica de manera inequívoca el nombre de la máquina hasheado.

![](<../../.gitbook/assets/Captura de pantalla 2021-12-22 a las 15.55.54.png>)

Adicionalmente contamos con otro forma de hacer llegar los logs al cliente en este caso en tiempo real. En este [enlace](https://docs.transparentedge.eu/guias/streaming-de-logs-con-kafka) puedes ver más detalles sobre como configurar esta funcionalidad.

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [dashboard](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../faq/glosario/api.md).
{% endhint %}

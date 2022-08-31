# Envío de logs

El envío de logs está incluído en cualquier paquete de servicio, simplemente se debe activar y configurar.

### Formato y perioricidad

En el modo básico los logs se envían cada hora en ficheros comprimidos y normalmente llegará un fichero por cada máquina (_edge_) que sirva al dominio en cuestión, siempre que haya tenido algo de tráfico.

El nombre de los ficheros tiene el siguiente formato:

```shell
2021072913-XX-france-b3fc576dcd50e32869c5873349ca5e7c.log.gz
```

* Los 10 primeros dígitos corresponden con la fecha del envío
* El siguiente campo 'XX' es un identificador numérico de cliente
* Después el país donde se encuentra el _edge_ que sirvió dichas peticiones
* Por último un _hash_ md5 que identifica inequívocamente al _edge_ que sirvió la petición (información interna)

### Envío a un FTP/sFTP

En esta opción se pueden hacer llegar los logs a un FTP/sFTP arbitrario.

Para configurarlo, simplemente se debe seleccionar el protocolo adecuado e introducir los datos requeridos, una vez activado y validado empezarán a llegar los _logs_ en el envío programado siguiente.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### Envío de logs Amazon S3 Compatible



Ofrece la opción de enviar los _logs_ directamente a un _bucket_ de S3.

Para poder configurarlo se necesitan credenciales con acceso programático y una política asociada que permita como mínimo realizar subidas en el _bucket_ a utilizar.

El _bucket_ a especificar debe ser su nombre, tal cual se ha configurado en S3 (no la URL), dicho nombre es el que se usó al crear el bucket en S3, las reglas de nombrado están [aquí](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html).

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### Logs en streaming

Adicionalmente contamos con otro forma de hacer llegar los logs al cliente en este caso en tiempo real. En este [enlace](https://docs.transparentedge.eu/guias/streaming-de-logs-con-kafka) puedes ver más detalles sobre como configurar esta funcionalidad.

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [dashboard](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../faq/glosario/api.md).
{% endhint %}

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

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt=""><figcaption></figcaption></figure>

### Envío de logs Amazon S3 Compatible

Esta opción permite enviar los _logs_ directamente a un _bucket_ S3 compatible.

Para poder configurarlo se necesitan credenciales con acceso programático y una política asociada que permita, como mínimo realizar subidas al _bucket_ a utilizar.

El campo "_Bucket_" acepta dos formatos:

* **\<ENDPOINT\_URL**_**>/<**_**BUCKET\_NAME>**
* **\<BUCKET\_NAME>** (Unicamente para AWS S3)

Donde `ENDPOINT_URL` es la _URL_ que aloja el servicio S3, si el _bucket_ está alojado en AWS S3 se puede omitir y únicamente especificar `BUCKET_NAME`, si se desea incluir igualmente (por ejemplo, para apuntar a una región específica), se debe extraer del domino el nombre del _bucket_, por ejemplo `https://tcdn-testing.s3.us-east-1.amazonaws.com/tcdn-testing` se transformaría en `https://s3.us-east-1.amazonaws.com/tcdn-testing`.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### Categorizar logs por fecha en el destino

A la hora de configurar el envío de logs, el campo "_Path_" (que es la ruta remota donde se almacenarán los logs) admite unas máscaras especiales que se expanden con la fecha actual:

* `%Y` - año, eg: 2022
* `%m` - mes, eg: 09
* `%d` - día, eg: 24
* `%H` - hora, eg: 13

Por ejemplo, si en el campo "_Path_" se especifica el valor `/tcdn-logs/%Y/%m/%d` y hoy es 24 de Agosto de 2022, cuando se ejecute el envío de logs, los ficheros serán almacenados remotamente en la ruta `/tcdn-logs/2022/08/22/*.gz`.

### Logs en streaming

Adicionalmente contamos con otro forma de hacer llegar los logs al cliente en este caso en tiempo real. En este [enlace](https://docs.transparentedge.eu/guias/streaming-de-logs-con-kafka) puedes ver más detalles sobre como configurar esta funcionalidad.

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [dashboard](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../faq/glosario/api.md).
{% endhint %}

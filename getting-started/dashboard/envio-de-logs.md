# Envío de logs

El envío de _logs_ está incluido en cualquier paquete de servicio. Simplemente se debe activar y configurar.

### Formato y periodicidad

En el modo básico, los _logs_ se envían cada hora en ficheros comprimidos. Por lo general llega un fichero por cada máquina (_edge_) que sirve al dominio en cuestión, siempre que haya tenido algo de tráfico.

El nombre de los ficheros tiene el siguiente formato:

```shell
2021072913-XX-france-b3fc576dcd50e32869c5873349ca5e7c.log.gz
```

* Los diez primeros dígitos corresponden con la fecha del envío.
* El siguiente campo, XX, es un identificador numérico de cliente.
* Después aparece el país donde se encuentra el _edge_ que sirvió dichas peticiones.
* Por último, vemos un _hash_ md5 que identifica inequívocamente al _edge_ que sirvió la petición (información interna).

### Envío a un FTP/sFTP

En esta opción se pueden hacer llegar los _logs_ a un FTP/sFTP arbitrario.

Para configurarlo, simplemente se debe seleccionar el protocolo adecuado e introducir los datos requeridos. Una vez activado y validado, empezarán a llegar los _logs_ en el envío programado siguiente.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2022-12-22 a las 16.11.06.png" alt=""><figcaption></figcaption></figure>

### Envío de _logs_ Amazon S3 Compatible

Esta opción permite enviar los _logs_ directamente a un _bucket_ compatible con S3.

Para poder configurarlo se necesitan credenciales con acceso programático y una política asociada que permita, como mínimo, realizar subidas al _bucket_ que vamos a utilizar.

El campo "_Bucket_" acepta dos formatos:

* **\<ENDPOINT\_URL**_**>/<**_**BUCKET\_NAME>**
* **\<BUCKET\_NAME>** (únicamente para AWS S3)

`ENDPOINT_URL` es la _URL_ que aloja el servicio S3. Si el _bucket_ está alojado en AWS S3, podemos utilizar únicamente el `BUCKET_NAME` como parámetro. Si se desea incluir igualmente, por ejemplo para apuntar a una región específica, se debe extraer del domino el nombre del _bucket:_  `https://tcdn-testing.s3.us-east-1.amazonaws.com/tcdn-testing` se transformaría en `https://s3.us-east-1.amazonaws.com/tcdn-testing.` Si no se hace así, el nombre del _bucket_ se incluirá al principio de la ruta remota.

<figure><img src="../../.gitbook/assets/Clipboard - December 22, 2022 1_43 PM (1).png" alt=""><figcaption></figcaption></figure>

### Categorizar logs por fecha y hora en destino

A la hora de configurar el envío de _logs_, el campo "_Path_", que es la ruta remota donde se almacenarán los _logs,_ admite unas máscaras especiales que se expanden con la fecha actual:

* `%Y` - año, eg: 2022
* `%m` - mes, eg: 09
* `%d` - día, eg: 24
* `%H` - hora, eg: 13

Por ejemplo, si en el campo "_Path_" se especifica el valor `/tcdn-logs/%Y/%m/%d` y hoy es 24 de agosto de 2022, cuando se ejecute el envío de _logs_, los ficheros serán almacenados remotamente en la ruta `/tcdn-logs/2022/08/22/*.gz`.

### _Logs_ en _streaming_

Adicionalmente contamos con otra forma de hacer llegar los _logs_ al cliente, en este caso en tiempo real. En este [enlace](https://docs.transparentedge.eu/guias/streaming-de-logs-con-kafka) puedes ver más detalles sobre cómo configurar esta funcionalidad.

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [dashboard](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../faq/glosario/api.md).
{% endhint %}

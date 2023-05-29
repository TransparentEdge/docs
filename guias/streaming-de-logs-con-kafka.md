---
description: Documentación y una guía rápida de cómo echar a andar tu primer consumer
---

# Logs en streaming

{% hint style="info" %}
Si tienes cualquier duda sobre el servicio de _Streaming de logs en tiempo real_, contacta con nosotros en [support@transparentedge.eu](mailto:soporte@transparentedge.eu)
{% endhint %}

Para activar el servicio de _Streaming de logs en tiempo real_, tan sólo debes acceder al panel en [https://dashboard.transparentcdn.com/](https://dashboard.transparentcdn.com/) y dentro del apartado _Logs_, en la pestaña _Streaming_, encontrarás todo lo necesario.

Una vez activado el servicio, **podrás descargarte un archivo zip** que contiene los certificados digitales necesarios para autenticar a tus _consumers_ así como una serie de plantillas preconfiguradas con tus datos para consumir los logs mediante _filebeat_, _logstash_ y _python_.

Además, **puedes agregar fácilmente las direcciones IP** en las cuales instalarás el/los _consumer_, para que  automáticamente se ajusten las reglas del _firewall_ necesarias en nuestros _brokers_.

Las plantillas, vienen preconfiguradas con todos los datos necesarios, pero a continuación, vamos a enumerar el contenido del zip y ciertos requisitos y parámetros importantes a considerar, uno muy importante es el Consumer Group.

#### Parámetros de conexión:

* La dirección de nuestros brokers:
  * `kafka1.edgetcdn.io`
  * `kafka2.edgetcdn.io`
  * `kafka3.edgetcdn.io`
* El puerto a utilizar, que será el `9093`

#### Contenido del zip:

* Certificado público de cliente **`c<ID>.crt.pem`**
* Certificado privado **`c<ID>.key.pem`**
* Keystore en formato PKCS12 **`c<ID>.keystore.p12`**
* La contraseña utilizada para cifrar el _keystore_ y el _truststore:_ **`password.txt`**
* El certificado público de nuestra CA **`transparentcdnCA.pem`**
* _Truststore_ con nuestra CA (necesario en algunos consumers): **`truststore.p12`**
* Plantilla para _Filebeat_: **`filebeat.yml`**
* Plantilla para _Logstash_: **`kafka-logstash.conf`**
* Consumer sencillo en _Python_: **`consumer.py`**

#### Otros datos:

* El _Topic_ al que suscribirse que será **`c<ID>`**
* **El prefijo de **_**Consumer Group**_** al que unir tus **_**consumers.**_ Por ejemplo, si tu `<ID>` (identificador de cliente) es el `83`, te suscribirás al topic `c83`, y podrás unir tus _consumers_ a cualquier _"Consumer Group"_ que comience por `c83_` como por ejemplo `c83_group1`, `c83_test`, `c83_pre` ... Puedes consultar más información sobre los consumer groups [aquí](https://docs.transparentcdn.com/guias/streaming-de-logs-con-kafka#que-son-los-consumer-groups).

#### Nosotros necesitaremos:

* La(s) dirección(es) IP(s) desde donde conectarán tus consumers. Que las puedes añadir desde el propio panel (al añadirlas, se necesita un margen de 5 minutos hasta que estén activas en nuestro _firewall_):

![](<../.gitbook/assets/image (20).png>)

## Consumiendo los logs

En la actualidad existen multitud de destinos para tus logs, te podría interesar el ingestarlos en un elasticsearch para hacer análitica de datos, o tal vez subirlos a un servicio de terceros como Datadog o Amazon S3, las opciones son casi infinitas y va a depender mucho de tus necesidades de negocio.&#x20;

Es por esto que, siendo fieles a nuestra filosofia de hacer las cosas lo más simples posible,  te vamos a proponer que uses dos herramientas ampliamente utilizadas en la comunidad como son [Filebeat](https://www.elastic.co/es/beats/filebeat) o/y [Logstash](https://www.elastic.co/es/logstash) para consumir tus logs de nuestro sistema de Logs en Streaming.

### Filebeat vs Logstash

Es muy común, sobre todo para gente que no está familiarizada con este tipo de tecnologías el confundir cuando usar Logstash, cuando usar Filebeat o cuando usarlos juntos que también se puede. Aquí vamos a intentar explicarlo de una manera un poco somera pero lo suficientemente sencilla como para poder tomar una decisión al respecto.

Logstash es un programa escrito en java parte del stack ELK (ElasticSearch - Logstash - Kibana) desarrollado y mantenido por la compañía ElasticSearch.&#x20;

Filebeat sin embargo está escrito en Go por la misma compañía y surgio como respuesta a una necesidad incipiente de la comunidad de tener una herramienta  ligera para transportar logs ya que logstash consume bastante más que Filebeat al estar escrito en java.

Filebeat como digo es un software muy liviano que te permite transportar logs de un sitio a otro, lo mismo que Logstash (este segundo no tan liviano), sin embargo, Logstash es mucho más versatil y potente de Filebeat y te permite consumir logs (Inputs) de un número mayor de sitios y enviarlos también a un número mayor de salidas (Ouputs).

Aquí os dejo los enlaces a los Input y Output de Logstash y Filebeat

* Filebeat ([Input](https://www.elastic.co/guide/en/beats/filebeat/current/configuration-filebeat-options.html), [Output](https://www.elastic.co/guide/en/beats/filebeat/current/configuring-output.html))
* Logstash ([Input](https://www.elastic.co/guide/en/logstash/current/input-plugins.html), [Output](https://www.elastic.co/guide/en/logstash/current/output-plugins.html))

Por tanto usar Logstash o Filebeat para sacar los logs de nuestro sistema de Logs en Streaming va a depender de tus necesidades, principalmente de el destino final de los mismos, logs por segundo y si quieres hacer algún tipo de transformación con los mismos.

Resumiendo, nuestra recomendación es que uses Filebeat siempre que puedas ya que es más ligero y fácil de configurar. Si necesitas alguna salida que no está en Filebeat o hacer alguna transformación usa Logstash.

Recuerda que siempre tendrás una tercera opción que también es válida y contemplamos en esta documentación y es que escribas tu propio consumer usando tu lenguaje de programación favorito. &#x20;

### Consumir logs mediante Filebeat

Veamos ahora un despliegue sencillo de _Filebeat_ en un servidor _Debian_ como primera toma de contacto en que vamos a dejar el log en un fichero de texto.

La documentación oficial se puede encontrar en: [https://www.elastic.co/es/beats/filebeat](https://www.elastic.co/es/beats/filebeat)

Usaremos estos datos de ejemplo pero recuerda que el zip que descargaste tras activar el servicio ya contiene una plantilla con todos los datos necesarios llamada **`filebeat.yml`**

* Certificados `c83.crt.pem` y `c83.key.pem`
* Contraseña: `password`
* Topic: `c83`
* Consumer group: `c83_filebeat`

Primero descargamos e instalamos el paquete de _Filebeat_ en nuestro servidor:

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.9.0-amd64.deb
sudo dpkg -i filebeat-7.9.0-amd64.deb
```

Habilitamos el módulo de Kafka:

```
filebeat modules enable kafka
filebeat setup -e
```

Editamos la configuración de _Filebeat_ `/etc/filebeat/filebeat.yml` y pegamos los datos que tenemos en la plantilla **`filebeat.yml`**, necesitarás editar los siguientes parámetros si copias los certificados en ubicaciones distintas o si modificas la ruta donde se volcarán los ficheros:

* `ssl.certificate:` Ubicación de `c<ID>.crt.pem`
* `ssl.key:` Ubicación de `c<ID>.key.pem`
* `ssl.certificate_authorities:` Ubicación de `transparentcdnCA.pem`
* `path:` Ruta final donde se depositarán los logs que consuma _Filebeat_.

```
filebeat.inputs:
- type: kafka
  hosts:
    - kafka1.edgetcdn.io:9093
    - kafka2.edgetcdn.io:9093
    - kafka2.edgetcdn.io:9093
  topics: ["c83"]
  group_id: "c83_filebeat"
  initial_offset: "newest"
  ssl.enabled: yes
  ssl.certificate: "/etc/filebeat/secret/c83.crt.pem"
  ssl.key: "/etc/filebeat/secret/c83.key.pem"
  ssl.key_passphrase: "password"
  ssl.certificate_authorities:
    - /etc/filebeat/secret/transparentcdnCA.pem

output.file:
  codec.format:
    string: '%{[message]}'
  path: "/tmp/filebeat"
  filename: kafka-filebeat.out
  rotate_every_kb: 50000
```

En el servidor donde configuremos _Filebeat_, copiamos la clave pública y privada del certificado así como la CA de Transparent Edge Services a las rutas que hayamos definido finalmente en la configuración.

También deberás crear la carpeta definida en el `path:` en el caso de que no exista.

Una vez configurado todo, tan sólo tendrás que iniciar el servicio de _Filebeat_, normalmente mediante _systemd_ con `systemctl start filebeat`, y si todo ha ido bien, verás como se empiezan a consumir los logs en la ruta que hayas definido en `path:`&#x20;

```
root@filebeat:/tmp/filebeat# ls -lrt
total 4
-rw------- 1 root root     49M ago 27 08:43 kafka-filebeat.out.7
-rw------- 1 root root     49M ago 27 08:43 kafka-filebeat.out.6
-rw------- 1 root root     49M ago 27 08:43 kafka-filebeat.out.5
-rw------- 1 root root     49M ago 27 08:44 kafka-filebeat.out.4
-rw------- 1 root root     49M ago 27 08:44 kafka-filebeat.out.3
-rw------- 1 root root     49M ago 27 08:45 kafka-filebeat.out.2
-rw------- 1 root root     49M ago 27 08:51 kafka-filebeat.out.1
-rw------- 1 root root    4,6M ago 27 08:52 kafka-filebeat.out
```

### Consumir logs mediante Logstash

Ahora veamos cómo consumir nuestros logs mediante _Logstash._

{% hint style="info" %}
Nota: Usaremos el _Keystore_ y el _Truststore_ en lugar del par de claves privada y pública, tendrás que copiarlos al servidor donde ejecutes _Logstash_. Además, el servicio de _Systemd_ por defecto utiliza el usuario _Logstash_, por lo que éste deberá tener permisos de lectura sobre ellos. Para el ejemplo los dejaremos en `/etc/logstash/certs`.
{% endhint %}

Instalaremos _Logstash_ desde la paquetería oficial, ejecuta los siguientes comandos para agregar el repositorio e instalar Logstash. O bien sigue la guía oficial en [https://www.elastic.co/guide/en/logstash/current/installing-logstash.html](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
apt install apt-transport-https

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
apt update
apt install logstash
```

Ahora configuraremos un _pipeline_, que consumirá los logs desde nuestros servidores de Kafka y los volcará en un fichero en formato JSON, categorizando cada campo del log.

{% hint style="info" %}
Recuerda que _Logstash_ ofrece múltiples entradas/salidas a distintos sistemas y te permite personalizar y mutar los logs, más info en: ([Input plugins](https://www.elastic.co/guide/en/logstash/current/input-plugins.html), [Output plugins](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)).
{% endhint %}

Para ello crea un fichero nuevo en `/etc/logstash/conf.d/kafka-logstash.conf` con el contenido que has recibido en la plantilla `kafka-logstash.conf` en el zip que descargaste desde el panel, necesitarás editar los siguientes parámetros si copias los certificados en ubicaciones distintas o si modificas la ruta donde se volcarán los ficheros:

* `ssl_keystore_location:` Ubicación de `c<ID>.keystore.p12`
* `ssl_truststore_location:` Ubicación de `truststore.p12`
* `path => :` Ruta al fichero donde se volcarán los logs.

```
input {
  kafka {
    bootstrap_servers => "kafka1.edgetcdn.io:9093,kafka2.edgetcdn.io:9093,kafka2.edgetcdn.io:9093"
    topics => "c83"
    group_id => "c83_logstash"
    auto_offset_reset => "latest"
    security_protocol => "SSL"
    ssl_keystore_location => "/etc/logstash/certs/c83.keystore.p12"
    ssl_keystore_password => "password"
    ssl_truststore_location => "/etc/logstash/certs/truststore.p12"
    ssl_truststore_password => "password"
  }
}

filter {
  grok {
    match => {
        "message" => [
            "%{DATA:clientip} - %{DATA:user} \[(.*)\] \"%{WORD:verb} %{DATA:request} %{DATA:httpversion}\" %{NUMBER:statuscode} %{DATA:bytes} \"%{DATA:useragent}\" %{DATA:hitmiss} \"%{DATA:content-type}\" \"%{DATA:layer}\" %{NUMBER:requesttime} \"%{DATA:clientid}\" \"%{DATA:referer}\" %{DATA:forwardedproto} %{DATA:country}(.*)",
            "%{DATA:clientip} - %{DATA:user} \[(.*)\] %{DATA:vod_host} \"%{WORD:verb} %{DATA:request} %{DATA:httpversion}\" %{NUMBER:statuscode} %{DATA:bytes} \"%{DATA:referer}\" \"%{DATA:useragent}\" \"%{DATA:content-type}\" \"%{DATA:hitmiss}\" \"%{DATA:layer}\" \"%{DATA:clientid}\" %{NUMBER:requesttime} %{DATA:forwardedproto} %{DATA:country}(.*)",
            "%{DATA:clientip} - %{DATA:user} \[(.*)\] %{DATA:vod_host} \"%{WORD:verb} %{DATA:request} %{DATA:httpversion}\" %{NUMBER:statuscode} %{DATA:bytes} \"%{DATA:referer}\" \"%{DATA:useragent}\" \"%{DATA:content-type}\" \"%{DATA:hitmiss}\" \"%{DATA:layer}\" \"%{DATA:clientid}\" %{NUMBER:requesttime} %{DATA:forwardedproto}(.*)"
        ]
    }
  }
  date {
    match => [ "timestamp" , "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
  mutate { remove_field => [ "message" ] }
}

output {
 file {
   path => "/var/log/tcdn_streaming/logstash.out"
   codec => json_lines
 }
}
```

Ya que el servicio se ejecutará con el usuario _logstash_, nos aseguramos de tener los permisos correctos, tanto para los certificados, como la configuración, como el directorio final donde escribirá _logstash:_

```
chown logstash:logstash -R /etc/logstash/
mkdir /var/log/tcdn_streaming
chown logstash:logstash /var/log/tcdn_streaming
```

Ahora sólo resta iniciar _Logstash_, podemos hacerlo por línea de comandos para verificar que todo funciona con:

```
sudo -u logstash /usr/share/logstash/bin/logstash "--path.settings" "/etc/logstash"
```

Recuerda verificar que todos los archivos necesarios son accesibles por el usuario "logstash", incluyendo los certificados, y el fichero + carpeta de destino.&#x20;

Al cabo de unos segundos, comenzarás a recibir los logs en el fichero o salida que hayas indicado en formato JSON, te dejamos un ejemplo:

```yaml
root@logstash:/var/log/tcdn_streaming# tail -1 logstash.out | jq
{
  "requesttime": "0.000193",
  "hitmiss": "hit",
  "clientid": "83",
  "verb": "GET",
  "content-type": "application/javascript",
  "httpversion": "HTTP/1.1",
  "bytes": "621",
  "request": "http://www.ejemplo.com/build/js/Core/Core.3160595ce5a674e1205b409c3e53616c4b44a9b3.js",
  "referer": "https://referer.ejemplo.com/",
  "user": "-",
  "forwardedproto": "https",
  "clientip": "11.11.222.111",
  "@version": "1",
  "statuscode": "200",
  "layer": "L1",
  "useragent": "Mozilla/5.0 (iPhone; CPU iPhone OS 13_7 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1.2 Mobile/15E148 Safari/604.1",
  "@timestamp": "2020-09-14T12:44:19.475Z"
}
```

Si todo ha ido bien, puedes cancelar el comando anterior y dejar el servicio ejecutándose con:

```
systemctl start logstash.service
```

### Consumir logs mediante un script de Python a medida

Se puede programar un consumer en muchos lenguajes distintos, vamos a ver un ejemplo con Python, utilizando el cliente o librería de "confluent-kafka-python" [https://github.com/confluentinc/confluent-kafka-python](https://github.com/confluentinc/confluent-kafka-python)

Otra opción también muy popular es [https://github.com/dpkp/kafka-python](https://github.com/dpkp/kafka-python)

Vamos con el ejemplo (nuevamente, usaremos _Debian_ como sistema operativo):

Instalamos los paquetes necesarios:

```
sudo apt install python3-pip
pip3 install confluent-kafka --user
```

Creamos el fichero python con el contenido que recibiste en la plantilla `consumer.py`, abajo se muestran unos datos de ejemplo, la parte importante estaría en el apartado de configuración, necesitarás editar los siguientes parámetros si copias los certificados en ubicaciones distintas:

* `ssl.ca.location:` Ubicación de `transparentcdnCA.pem`
* `ssl.keystore.location:` Ubicación de `c<ID>.keystore.p12`

```python
#!/usr/bin/python3
import sys
import logging
import socket
from confluent_kafka import Consumer

LOG_FMT = '[%(asctime)s][%(levelname)s] %(message)s'
logging.basicConfig(
    stream=sys.stdout,
    format=LOG_FMT,
    datefmt='%Y.%m.%d %H:%M:%S',
    level=logging.INFO)

def assigned(consum, parti):
    logging.info("Assigned consumer: %s on partition %s", repr(consum), repr(parti))

def revoked(consum, parti):
    logging.error("Failed to assign consumer: %s on partition %s", repr(consum), repr(parti))

if __name__ == "__main__":
    # CONFIGURACION
    SERVERS = 'kafka1.edgetcdn.io:9093,kafka2.edgetcdn.io:9093,kafka3.edgetcdn.io:9093'
    TOPIC = 'c83'
    CONF = {'bootstrap.servers': SERVERS,
            'client.id': socket.gethostname(),
            'security.protocol': 'SSL',
            'ssl.ca.location': '/usr/local/share/ca-certificates/transparentcdnCA.pem',
            'ssl.keystore.location': '/root/secret/c83.keystore.p12',
            'ssl.keystore.password': 'password',
            'group.id': TOPIC + '_python'}
    # FIN CONFIGURACION

    consumer = Consumer(CONF)
    consumer.subscribe([TOPIC], on_assign=assigned, on_revoke=revoked)
    count = 0
    try:
        while True:
            msg = consumer.poll(1.0)
            if msg is None:
                #logging.info("Waiting for message or event/error in poll()")
                continue

            if msg.error():
                logging.error(msg.error())
            else:
                value = msg.value()
                offset = msg.offset()
                topic = msg.topic()
                logging.info("Mensaje recibido numero {} - Value: \"{}\" - Topic: {} - Offset: {}".format(
                    str(count),
                    value.decode('utf-8'),
                    str(topic),
                    str(offset)))

                count += 1

    except KeyboardInterrupt:
        pass
    finally:
        consumer.close()

```

Si todo es correcto, al iniciar el consumer recibirás el siguiente mensaje y empezarás a consumir del topic:

```
root@server1:~# ./consumer.py
[2020.08.21 09:57:21][INFO] Assigned consumer: <cimpl.Consumer object at 0x7fada8213f28> on partition [TopicPartition{topic=c83,partition=0,offset=-1001,error=None}, TopicPartition{topic=c83,partition=1,offset=-1001,error=None}]
```

## ¿Qué son los Consumer Groups?

Tradicionalmente los Brókers de mensajería (Message Brokers), actuaban de dos formas:

* **Queue**: El mensaje se publica una vez, se consume una vez.
* **Pub/Sub**: El mensaje se publica una vez, se consume múltiples veces.

Kafka puede funcionar de las dos formas gracias a los _Consumer Groups:_

* Si queremos actuar como una cola (Queue), ponemos todos los _consumers_ en el mismo _consumer group_.
* Si queremos actuar como un Pub/Sub, cada _consumer_ va en un grupo distinto.

### Ejemplos

Actualmente, creamos los _topics_ con 2 particiones por defecto (se pueden aumentar si se solicita), vamos a ver unos ejemplos con un topic de 2 particiones:

* Iniciamos 3 _consumers_, los 3 en el **mismo **_**consumer group**_: Uno de ellos, consume de la partición 0, otro de la 1, y el último queda totalmente parado. Conseguimos procesamiento en paralelo y alta disponibilidad. (La alta disponibilidad también la logramos con 2 _consumers_, si uno falla, el otro consumirá de las dos particiones). Los mensajes estarán repartidos entre el _consumer 1_ y el _consumer 2_.

![](<../.gitbook/assets/image (16).png>)

* Iniciamos 2 _consumers_, cada uno en **distinto **_**consumer group**_: Los 2 van a recibir TODOS los mensajes del topic y estarán totalmente aislados. Es útil si queremos realizar un procesamiento distinto de los mensajes recibidos para cada uno de ellos. A este esquema podemos añadir más _consumers_, el resultado será el mismo, cada uno de ellos recibirá todos los mensajes de todas las particiones.

![](<../.gitbook/assets/image (54).png>)

Dado que trabajamos con logs y a no ser que se requieran varios post-procesos distintos, lo más interesante es tener los _consumers_ en el mismo _consumer group_, y **es más que probable que solamente un **_**consumer**_** sea suficiente dado el rendimiento que ofrece Kafka**. Se pueden iniciar mas _consumers_ si uno de ellos no puede consumir en tiempo real, o si queremos procesamiento en paralelo + alta disponibilidad.

## Consumir los logs del WAF

Si tienes activo el servicio [WAF](https://docs.transparentedge.eu/security/waf), también puedes consumir los logs del _audit_ en tiempo real. Estos logs, a diferencia del servicio de delivery se encuentran en formato JSON.

Se puede utilizar, por ejemplo, el consumer en Python [comentado anteriormente](https://docs.transparentedge.eu/guias/streaming-de-logs-con-kafka#consumir-logs-mediante-un-script-de-python-a-medida), la única diferencia será el _topic_ al que nos suscribiremos, que en este caso tendrá este formato: `c<ID>_waf`.

Por ejemplo, si tu compañia tiene el `<ID>` (identificador de cliente) 83, deberás suscribirte al _topic_ `c83_waf`.

### Formato de los logs del Audit

El formato del servicio de WAF es un objeto JSON estándar.

Dicho JSON contiene **todos** los datos relevantes de la request: el código http, las cabeceras de respuesta, las cabeceras de la petición, la URL, el método, la IP del cliente ... básicamente toda la información. Y además, contiene un campo con todos los detalles relacionados con el ataque detectado.

{% hint style="info" %}
Se hace referencia a los campos separándolos por un punto, ya que es la notación que se utiliza en [jq](https://stedolan.github.io/jq/), un procesador JSON. Por ejemplo, si hacemos referencia al campo `.transaction.messages.message` en `jq`, lo mismo en Python sería:`["transaction"]["messages"]["message"]`
{% endhint %}

Una sola request, puede hacer que salten una o varias reglas del WAF, es por ello que el campo `.transaction.messages`, es un _array_ de diferentes mensajes.&#x20;

Para facilitar el tratamiento de los logs, desde la CDN os los enviamos separando cada ataque, por lo tanto, el campo `.transaction.messages` deja de ser un _array_ y se transforma en un único objeto json que contiene la información de un sólo ataque.

Es decir, si un usuario malintencionado realiza una petición POST a `/wp-admin` y desde la CDN detectamos 2 ataques en esa misma petición, recibiréis 2 logs.

Aquí accedemos al campo "messages" utilizando `jq`, pero perfectamente lo podríamos hacer en Python u otros lenguajes. Revisamos el campo "message" del ataque detectado para cada requests.

```bash
$ tail -3 audit.log | jq '.transaction.messages.message'
"Host header is a numeric IP address"
"Possible Remote File Inclusion (RFI) Attack: URL Payload Used w/Trailing Question Mark Character (?)"
"XSS Attack Detected via libinjection"
```

Uno de los campos importantes que contiene `messages` es `ruleId`. Se trata de un valor numérico que nos permitirá establecer excepciones.

Este es un ejemplo del contenido del campo `messages`:

```bash
{
  "message": "Mensaje descriptivo del ataque.",
  "details": {
    "match": "Mensaje técnico de porqué este ataque ha hecho saltar la regla.",
    "ruleId": "931120",
    "file": "ruta al fichero que contiene la regla 931120",
    "lineNumber": "78",
    "data": "Mensaje técnico de los datos concretos en la requests que hicieron saltar la regla.",
    "severity": "2",
    "ver": "OWASP_CRS/3.3.2",
    "rev": "",
    "tags": [
      "application-multi",
      "language-multi",
      "platform-multi",
      "attack-rfi",
      "paranoia-level/1",
      "OWASP_CRS",
      "capec/1000/152/175/253"
    ],
    "maturity": "0",
    "accuracy": "0"
  }
}
```

# Formato de logs

Transparent Edge Services pone los logs de sus sitios web a disposición de los clientes. Hay dos opciones de funcionamiento:

1\. (PULL) El cliente puede recoger los logs en un FTP habilitado para tal propósito.

2\. (PUSH) Transparent Edge Services envía los logs de forma horaria a un FTP que el cliente pone para tal propósito.

Este servicio no viene activado por defecto y su activación puede realizarse como se indica [aquí](../dashboard/envio-de-logs.md).

### Formato de los logs - Servicio Web Caching

El log puede tener dos posibles formatos, dependiendo si viene de la primera capa de cachés (L1) o de la segunda (L2):[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Obtenci%C3%B3n\_de\_logs/edit?section=3)

## L1

```
139.47.132.74 - - [25/May/2020:12:56:34 +0000] "GET http://www.example.com/css/i/blank.gif HTTP/1.1" 200 49 "Mozilla/5.0 (Web0S; Linux/SmartTV) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.34 Safari/537.36 HbbTV/1.4.1 (+DRM; LGE; 49UK6300PLB; WEBOS4.0 05.10.45; W4_LM18A; W4_LM18A;)" hit "image/gif" "L1" 169 "213" "http://www.example2.com/v2.3.1_1/index.html" http ES "OK"
```

* **Campo 1**: IP del cliente.
* **Campo 2**: Remote logname. Siempre '-'.
* **Campo 3**: Usuario desde auth basic.
* **Campo 4**: Fecha y hora (UTC).
* **Campo 5**: Metodo HTTP.
* **Campo 6**: Url solicitada.
* **Campo 7**: Protocolo.
* **Campo 8**: Status Code.
* **Campo 9:** Tamaño del objeto.
* **Campo 10**: User Agent.
* **Campo 11:** Resultado de la petición (HIT/MISS).
* **Campo 12**: Content Type.
* **Campo 13**: Capa de caché que ha servido la petición (L1/L2).
* **Campo 14**: Tiempo de respuesta de la _request_ en segundos con precisión de microsegundos, en microsegundos directamente en la plataforma de autoprovisión.
* **Campo 15**: Identificador de cliente (_client id_).
* **Campo 16**: Referer.
* **Campo 17**: Real protocol (puesto que deshacemos el https en una capa anterior a la de caché, este campo es necesario para identificar si la petición original ha sido pedida por http o por https).
* **Campo 18**: Country Code



## L2

```
144.178.141.169 - - [31/Mar/2016:17:36:42 +0200] "POST http://www.example.com/post.php?action=port_test_call HTTP/1.1" 200 66 "Mozilla/5.0 (Windows NT 6.3; WOW64; rv:54.0) Gecko/20100101 Firefox/54.0" miss "text/html; charset=UTF-8" "L2" 0.507524 "99" "https://www.example.ecom/test/" "TCDN-OK:301029833" https ES
```

* **Campo 1**: IP de cliente.
* **Campo 2**: Remote logname. siempre '-'.
* **Campo 3**: Usuario desde auth basic.
* **Campo 4:** Fecha y hora + diferencia horaria.
* **Campo 5**: Metodo HTTP.
* **Campo 6**: Url solicitada.
* **Campo 7:** Protocolo.
* **Campo 8**: Status Code.
* **Campo 9**: Tamaño del objeto.
* **Campo 10**: User Agent.
* **Campo 11**: Resultado de la petición (HIT/MISS).
* **Campo 12**: Content Type.
* **Campo 13**: Capa de caché que ha servido la petición. (L1/L2).
* **Campo 14**: Tiempo de respuesta de la _request_.
* **Campo 15**: Identificador de cliente (client id).
* **Campo 16**: Referer.
* **Campo 17**: [TCDN Debug](broken-reference). Código de interno de error de la respuesta del _backend_ del cliente.
* **Campo 18**: Real protocol (puesto que deshacemos el https en una capa anterior a la de caché, este campo es necesario para identificar si la petición original ha sido pedida por http o por https).
* **Campo 19:** Country Code.&#x20;

## Formato de los logs - Servicio Media

```
37.132.249.54 - - [31/Jul/2017:18:07:20 +0200] vod.example.com "GET /resources/1497635691910.mp4 HTTP/1.1" 200 153784 "-" "stagefright/1.2 (Linux;Android 4.4.2)" "video/MP2T" "HIT" "L1" "36" 0.024 http ES
```

* **Campo 1**: IP de cliente.
* **Campo 2**: Remote logname. Siempre '-'.
* **Campo 3**: Usuario desde auth basic.
* **Campo 4**: Fecha y hora + diferencia horaria.
* **Campo 5**: Cabecera de Host.
* **Campo 6**: Request URI.
* **Campo 7:** Protocolo.
* **Campo 8**: Status Code.
* **Campo 9:** Tamaño del objeto.
* **Campo 10**: Referer.
* **Campo 11**: User Agent.
* **Campo 12**: Content Type.
* **Campo 13:** Resultado de la petición (HIT/MISS/expired).
* **Campo 14**: Capa de cache que ha servido la petición.
* **Campo 15**: Client ID.
* **Campo 16**: Tiempo de respuesta de la _request_.
* **Campo 17**: Real protocol (puesto que deshacemos el https en una capa anterior a la de caché, este campo es necesario para identificar si la petición original ha sido pedida por http o por https).
* **Campo 18**: Country Code.

## Formato de los logs del WAF

```
47.62.117.136 - - [25/May/2020:15:03:22 +0000] "GET http://www.example.com/elt/ HTTP/1.0" 200 12423 "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36" "text/html" "waf" 1 "179"
```

* **Campo 1**: IP de cliente.
* **Campo 2**: Remote logname. Siempre '-'.
* **Campo 3**: Usuario desde auth basic.
* **Campo 4**: Fecha y hora + diferencia horaria.
* **Campo 5**: Cabecera de Host.
* **Campo 6**: Request URI.
* **Campo 7:** Protocolo.
* **Campo 8**: Status Code.
* **Campo 9:** Tamaño del objeto.
* **Campo 10**: User Agent.
* **Campo 11**: Content Type.
* **Campo 12**: Capa que ha servido la _request_ (en este caso **waf**).
* **Campo 13**: Tiempo de respuesta de la _request_.
* **Campo 14**: Client ID.

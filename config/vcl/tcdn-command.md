# TCDN-Command

En Transparent Edge Services usamos la cabecera `TCDN-Command` **** como una forma de invocar funciones preprogramadas por nosotros para hacerte la vida más fácil y que tus configuraciones sean más sencillas de realizar.

Este comando nos permite hacer ciertas cosas que, de otra manera, tendrías que hacer desde cero. Es importante que entiendas su uso, ya que no es algo trivial: un mal uso puede hacer que tu _web_ no se comporte como piensas que debe hacerlo.

Lo primero que debes entender es que es una cabecera _web_ y, si la vas a invocar varias veces y con distintas llamadas a funciones, tiene que ser concatenada para que su valor no sea sobrescrito. Por ejemplo:

```javascript
sub vcl_recv {
    if (req.http.True-Client-Ip ~ "(1.1.2.2|1.2.3.4)") {
        set req.http.TCDN-Command = "deny_request";
    }
    if ((req.http.host == "www.transparentedge.eu") && (req.url ~ "/my-new-url")) {
        set req.http.TCDN-Command = "pass, " + req.http.TCDN-Command;
    }
}
```

Como vemos, aquí llamamos en primer lugar a la función `deny_request` para _banear_ dos direcciones IP. Pero, además, queremos que las peticiones sobre la URL `http://www.transparentedge.eu/my-new-url` no sean cacheadas nunca; para ello, como ves, asignamos la cabecera `TCDN-Command` con el valor `pass`**.** Sin embargo, en la segunda llamada, en lugar de hacerlo como la primera vez, lo cual sobrescribiría el valor de la cabecera, concatenamos el contenido actual de la misma con el valor que queremos añadir:

```javascript
set req.http.TCDN-Command = "pass, " + req.http.TCDN-Command;
```

Nótese la importancia de la coma para separar de manera correcta ambos comandos.

Actualmente, la cabecera `TCDN-Command` soporta estos valores:

* `brotli_compress`
* [`deny_request`](../../security/bloqueando-por-direccion-ip.md)``
* [`limit_rate`](../../security/waf/limit\_rate.md)``
* [`pass`](../../getting-started/faq/forzando-no-cachear.md)``
* [`redirect_https`](../../getting-started/faq/funcionalidades/redirecciones.md#redirecion-a-https)
* [show-captcha](https://docs.transparentedge.eu/security/waf/captcha)
* [protect-with-token](https://docs.transparentedge.eu/security/waf/contenido-protegido-por-token)
* `vary-device`
* `vary-protocol`

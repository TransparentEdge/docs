# Bloqueando por dirección IP

Para bloquear tráfico que procede de una o varias direcciones IP es muy sencillo incorporando esto pequeño fragmento de configuración dentro de la función vcl\_recv.

```javascript
sub vcl_recv{
    if (req.http.True-Client-Ip ~ "(1.1.2.2|1.2.3.4)") { 
        set req.http.TCDN-Command = "deny_request, " + req.http.TCDN-Command;
    } 
}

```

Como vés, es autodescriptivo, por un lado usamos la dirección IP que llega en la cabecera [True-Client-IP](../getting-started/faq/cabeceras-por-defecto/true-client-ip-y-x-forwarded-for.md) y por otro, si request viene desde la IP listada en se bloqueará con un 403 mediante el seteo de la cabecera **req.http.TCDN-Command** con el valor "deny-request"&#x20;

Para entender el correcto funcionamiento de la cabecera TCDN-Command visita este [enlace](../config/vcl/tcdn-command.md).

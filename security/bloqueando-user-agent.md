# Bloqueando User-Agent

Bloquear un User-Agent es muy sencillo, solo tienes que añadir estas lineas a tu fichero de configuración dentro de la función vcl\_recv y adaptarla a tus necesidades.

```javascript
sub vcl_recv{
    if (req.http.User-Agent ~ "^curl") { 
        set req.http.TCDN-Command = "deny_request";
    } 
}
```

En este ejemplo todo lo que venga desde con un User-Agent que empiece por la palabra curl será denegado con 403. Este 403 es devuelto por al setear la cabecera **TCDN-Command** con el valor **deny\_request.**

Para entender el correcto funcionamiento de la cabecera TCDN-Command visita este [enlace](../config/vcl/tcdn-command.md).

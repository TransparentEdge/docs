# Reescritura de cabeceras

Al estar basados en [Varnish](https://www.varnish-software.com/), en particular en Varnish Plus, tenemos mucha flexibilidad a la hora de modificar el comportamiento por defecto de tu sitio web. La reescritura de cabeceras es algo muy común que se puede hacer sin demasiados problemas. Los casos más comunes son:

#### Reescritura de la cabecera de Host

El caso de uso típico.

```javascript
sub vcl_recv{
  if ((req.http.host == "www.transparentedge.eu") && (bereq.url ~ "/blog")) {
    set http.host == "blog.transparentedge.eu"
    set req.backend_hint = c83_tcdn.backend();
  }
}
```

#### Reescritura de la cabecera Cache-Control

```javascript
sub vcl_backend_response {
     if ((bereq.http.host == "www.transparentedge.eu") && (bereq.url ~ "/my-new-url")) {
            set beresp.http.Cache-Control = "max-age=0, s-maxage=10";
            set beresp.ttl = 10s;
    }
}
```

#### Borrado de cabeceras

Basándonos en el ejemplo anterior, podemos asegurarnos de borrar la cabecera Cache-Control que llega desde el origen antes de efectuar el forzado del tiempo de caché.

```javascript
sub vcl_backend_response {    
    if ((bereq.http.host == "www.transparentedge.eu") && (bereq.url ~ "/my-new-url")) {
            unset beresp.http.Cache-Control;
            set beresp.http.Cache-Control = "max-age=0, s-maxage=10";
            set beresp.ttl = 10s;
    }
}    
```








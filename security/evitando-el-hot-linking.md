---
description: Protección básica frente al hot-linking
---

# Evitando el Hot-linking

Aunque la mayoría de navegadores están evolucionando de cara a una mayor privacidad aplicando políticas por defecto más estrictas en cuanto al [Referrer-Policy,](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Referrer-Policy) aún se puede conseguir una protección básica frente al [_Hot-linking_](https://es.wikipedia.org/wiki/Hotlinking).

Para ello, tan sólo debes definir la cabecera `TCDN-Avoid-Hotlink-URL` con el _path_ al recurso que quieras servir como _placeholder_.

Por ejemplo, si quieres evitar que hagan _hot-linking_ con las imágenes que tenemos ubicadas en el path `/wiki/contenido` de tu dominio `www.example.com` , el código vcl a insertar en la configuración sería similar a:

```python
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if(req.url ~ "^/wiki/contenido" && urlplus.get_extension() ~ "^(jpg|jpeg|png|gif|svg|mp4)$") {
            set req.http.TCDN-Avoid-Hotlink-URL = "/img/hotlink-placeholder.png";
        }
    }
}
```

Como siempre, lo puedes definir en un nuevo bloque `vcl_recv` o en el ya existente.

Ahora, las peticiones contra esos recursos y con esas condiciones que tengan un _referer_ distinto al dominio del site actual, servirán en su lugar el placeholder `/img/hotlink-placeholder.png`. Es obligatorio definir un _placeholder_.

Se pueden agregar todas las condiciones necesarias al código anterior, por ejemplo, si el dominio `www.example2.com` puede realizar _hot-linking_ sin restricción alguna, el código quedaría así:

```python
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if(
                req.url ~ "^/wiki/contenido" &&
                urlplus.get_extension() ~ "^(jpg|jpeg|png|gif|svg|mp4)$" &&
                req.http.referer !~ "^https?://www.example2.com"
          ) {
            set req.http.TCDN-Avoid-Hotlink-URL = "/img/hotlink-placeholder.png";
        }
    }
}
```

{% hint style="warning" %}
Por defecto, se incluyen las siguientes excepciones:

* Por supuesto, si el _referer_ coincide con el dominio actual, no aplica.
* No es correcto definir `TCDN-Avoid-Hotlink-URL` con una cadena vacía o un path que no comience por "/", en el caso de hacerlo, se sustituirá por "/".
* Algunos _User-Agents_ están permitidos por defecto (buscadores y similares) para evitar perjudicar el posicionamiento.
* Se excluyen los _referers_ vacíos o que no incluyan el protocolo `http(s)`, debido al _Referrer Policy_ es poco práctico no hacerlo sin perjudicar al sitio web.
{% endhint %}

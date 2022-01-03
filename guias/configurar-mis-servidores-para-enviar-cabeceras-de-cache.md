# Configurar mis servidores para enviar cabeceras de cache

Vamos a explicar cómo configurar los servidores para enviar cabeceras de caché en **Apache** y **Nginx.**

### Apache

Para configurar cabeceras de caché con Apache podemos utilizar dos módulos: _mod\_expires_ y _mod\_header_

#### mod\_expires

Nos permite manipular la cabecera HTTP Expires y la directiva max-age del servidor. Con ello podemos configurar en cuanto tiempo expiran los recursos almacenados en caché. Este módulo se puede aplicar en los contextos de: server config, virtual host, directory y .htaccess.

**Ejemplo de configuración**

```
<ifModule mod_expires.c>
ExpiresActive On
ExpiresDefault "access plus 5 seconds" 
ExpiresByType image/x-icon "access plus 2592000 seconds" 
ExpiresByType image/jpeg "access plus 2592000 seconds" 
ExpiresByType image/png "access plus 2592000 seconds" 
ExpiresByType image/gif "access plus 2592000 seconds" 
ExpiresByType text/javascript "access plus 216000 seconds" 
ExpiresByType application/javascript "access plus 216000 seconds" 
ExpiresByType text/html "access plus 600 seconds" 
ExpiresByType application/xhtml+xml "access plus 600 seconds" 
</ifModule>
```

Para más información sobre mod\_expires pulsa [aquí](https://httpd.apache.org/docs/2.4/mod/mod\_expires.html).

#### mod\_headers

Nos proporciona directivas con las que podremos tener control sobre las respuestas de los encabezados. Podremos reemplazar, fusionar o eliminar las cabeceras de nuestra web. Este módulo podremos aplicarlo en los contextos: server config, virtual host, directory, .htaccess

**Ejemplo de configuración**

```
 # BEGIN Cache-Control Headers
<ifModule mod_headers.c>
  <filesMatch "\.(ico|jpe?g|png|gif|swf)$">
    Header set Cache-Control "public" 
  </filesMatch>
  <filesMatch "\.(css)$">
    Header set Cache-Control "public" 
  </filesMatch>
  <filesMatch "\.(js)$">
    Header set Cache-Control "private" 
  </filesMatch>
</ifModule>
# END Cache-Control Headers
```

Para más información sobre mod\_expires pulsa [aquí](http://httpd.apache.org/docs/2.2/mod/mod\_headers.html).

### Nginx

Para configurar cabeceras de caché en Nginx utilizaremos: **ngx\_http\_headers\_module**

Este módulo nos permite añadir directivas "Expires" y “Cache-Control” en una cabecera de respuesta. Podemos aplicarlo en los contextos: http, server, location y if in location

**Ejemplo de configuración**

```
expires    24h;
expires    modified +24h;
expires    @24h;
expires    0;
expires    -1;
expires    epoch;
expires    $expires;
add_header Cache-Control private;
```

Más información en [http://nginx.org/en/docs/http/ngx\_http\_headers\_module.html](http://nginx.org/en/docs/http/ngx\_http\_headers\_module.html)

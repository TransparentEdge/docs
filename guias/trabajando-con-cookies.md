# Trabajando con cookies

En Transparent Edge Services implementamos [Varnish Enterprise](https://www.varnish-software.com) y por lo tanto puedes aprovechar todas las ventajas que esto supone.

En este caso implementamos el módulo [cookieplus](https://docs.varnish-software.com/varnish-cache-plus/vmods/cookieplus/) de Varnish.

Este módulo te permite interactuar con las cookies tanto de la request como de las respuesta. Podrás añadir, quitar, sacar el valor de la cabecera Cookie (req.http.Cookie) o incluso de la cabecera Set-Cookies (resp.http.Set-Cookie).

Todas las funciones mantienen  un estado interno de la Cookie y el Set-Cookie hasta que lo liberes en la configuración mediante la función **write()** o **setcookie\_write().**

Las operaciones con set-cookies solo puedes usarse donde los objetos [resp o beresp](../config/vcl/vcl-objects.md) esté disponible (vcl\_deliver o vcl\_backend\_response)

### Filtrado de cookies

```javascript
sub vcl_recv
{
    // Keep these Cookies
    cookieplus.keep("JSESSIONID");
    cookieplus.keep_regex("^BNES_SSESS");
    cookieplus.keep_regex("^SSESS[a-zA-Z0-9]+$");
    cookieplus.keep_regex("^SESS[a-zA-Z0-9]+$");

    // Any Cookie that is not kept will be removed
    cookieplus.write();
}
```

### Filtrado de set-cookies

```javascript
sub vcl_backend_response
{
    // Mark all Set-Cookies for removal
    cookieplus.setcookie_keep("");

    // Only allow these Set-Cookies if the URL allows for it
    if (bereq.url ~ "^/admin") {
        cookieplus.setcookie_keep("JSESSIONID");
        cookieplus.setcookie_keep_regex("^BNES_SSESS");
        cookieplus.setcookie_keep_regex("^SSESS[a-zA-Z0-9]+$");
        cookieplus.setcookie_keep_regex("^SESS[a-zA-Z0-9]+$");
    }

    // Any Set-Cookie that is not kept will be removed
    cookieplus.setcookie_write();
}
```

Puedes encontrar una documentación completa del módulo cookieplus en este [enlace](https://docs.varnish-software.com/varnish-cache-plus/vmods/cookieplus/) de la documentación oficial de Varnish Enterprise.

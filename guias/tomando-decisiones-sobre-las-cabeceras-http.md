# Tomando decisiones sobre las cabeceras HTTP

Una de las ventajas de usar Varnish es su versatilidad, y está nos tomar decisiones en base a cualquier cabecera que nos llegue en la petición web.

Si bien antes veíamos cómo bloquear contenido por [IP](../security/bloqueando-por-direccion-ip.md) o por [User-Agent](../security/bloqueando-user-agent.md) es igualmente sencillo tomar decisiones de cualquier tipo basándoos en el valor o en la simple existencia de una cabecera http.

#### Cambiar de backend en función de la cabecera&#x20;

Al igual que [aquí](enrutando-el-trafico-a-distintos-backend.md) explicábamos como elegir un backend y otro en función de una url o host, es igualmente sencillo hacerlo en función de una cabecera por ejemplo la [geo\_country\_code](../getting-started/faq/funcionalidades/geolocalizacion-y-geobloqueo.md).

```javascript
sub vcl_recv{
    # Default backend
    set req.backend_hint = c82_tcdnes.backend();
    
    # Changing backend for Spanish users
    if (req.http.geo_country_code ~ "ES") { 
        set req.backend_hint = c82_tcdnes.backend();
    }
    
    # Changing backend for American users
    if (req.http.geo_country_code ~ "US") { 
        set req.backend_hint = c82_tcdnus.backend();
    }
    
}
```



#### Permitir tráfico solo a usuarios con una cabecera específica.

Esto es especialmente útil cuando cuando necesitas restringir el acceso a tu site por el motivo que sea pero no quieres protegerlo con [Basic Authenticacion ](../getting-started/faq/cosas-a-tener-en-cuenta.md)por qué eso haría que el site no se caché.

```javascript
sub vcl_recv{
    if (req.http.auth-tcdn != "e37be3f5e06e263445654c0d6ba0e123") {
        set req.http.TCDN-Command = "deny_request";
    }
}
```

Este mecanismo es muy útil pero necesitas que tu navegador meta esa cabecera http en cada una de las request para que tu navegación funcione. Para hacer esto te recomendamos instalar la extensión de Chrome [ModHeader](https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en).


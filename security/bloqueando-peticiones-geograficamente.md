# Bloqueando peticiones geográficamente

De manera predeterminada en Transparent Edge Services geolocalizamos todas las peticiones que pasan por nuestros sistemas, enviando a origen siempre una cabecera con el código del Pais desde el cual se hizo la petición. Esta cabecera es geo\_country\_code.

```
geo_country_code: ES
```

Para esta funcionalidad nos integramos con [GeoIP2 Enterprise Database](https://www.maxmind.com/en/solutions/geoip2-enterprise-product-suite/enterprise-database). Está base de datos tiene una precisión en cuanto a país del **99.8%.**&#x20;

Sería posible enviar otros datos que nos facilite esta base de datos y setearlo en forma de cabecera http para enviarla al origen como por ejemplo la cuidad, pero hay que tener en cuenta que la precisión a nivel de ciudad de esta base de datos está en torno al 75% para España. Puedes consultar la precisión por país [aquí.](https://www.maxmind.com/en/geoip2-city-accuracy-comparison)

El valor de la cabecera geo\_country\_code es el código del país en base al estándar [ISO 3166.](https://www.iso.org/obp/ui/#search)

Si el sistema no ha sido capaz de ubicar la ip del usuario envía el string 'Unknown' dentro de la cabecera.

A través de esta cabecera podemos en realidad, tomar cualquier decisión, como puede ser hacer una redirección, servir contenido específico para ese país o simplemente geobloquear un contenido.

Para esto último, nos iremos a nuestro [panel](https://dashboard.transparentcdn.com), a la parte de **Provisioning, VCL Config** y en la parte avanzada duplicaremos la configuración que está en producción en estos momentos e introduciremos el código de geobloqueo dentro de la función vcl\_recv:

```javascript
sub vcl_recv{
    if (req.http.geo_country_code ~ "RU") { 
        set req.http.TCDN-Command = "deny_request";
    } 
}

```

Para entender el correcto funcionamiento de la cabecera TCDN-Command visita este [enlace](../config/vcl/tcdn-command.md).

Del mismo modo podríamos redireccionar la web a una url o site específico en función de la procedencia del usuario, por ejemplo:

```javascript
sub vcl_recv{
    if (req.http.geo_country_code ~ "ES|AD|PT") { 
        set req.http.X-retSynth = "751,https://www.transparentcdn.com/es";
    } else {
        set req.http.X-retSynth = "751,https://www.transparentcdn.com/en"
    }
}
```

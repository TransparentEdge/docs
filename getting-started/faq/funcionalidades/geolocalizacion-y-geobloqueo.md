# Geolocalización y geobloqueo

De manera predeterminada, en Transparent Edge Services geolocalizamos todas las peticiones que pasan por nuestros sistemas, enviando a origen siempre una cabecera con el código del país desde el cual se hizo la petición. Esta cabecera es geo\_country\_code.

```
geo_country_code: ES
```

Para esta funcionalidad, nos integramos con [GeoIP2 Enterprise Database](https://www.maxmind.com/en/solutions/geoip2-enterprise-product-suite/enterprise-database). Esta base de datos tiene una precisión en cuanto a país del **99.8 %.**&#x20;

Sería posible enviar otros datos que nos facilite esta base de datos y setearlo en forma de cabecera http para enviarla al origen -por ejemplo, la ciudad-, pero hay que tener en cuenta que la precisión a nivel de ciudad de esta base de datos está en torno al 75 % para España. Puedes consultar la precisión por país [aquí.](https://www.maxmind.com/en/geoip2-city-accuracy-comparison)

El valor de la cabecera geo\_country\_code es el código del país en base al estándar [ISO 3166.](https://www.iso.org/obp/ui/#search)

Si el sistema no ha sido capaz de ubicar la IP del usuario, envía el _string_ 'Unknown' dentro de la cabecera.

Gracias a esta funcionalidad somos capaces de hacer multitud de cosas en función de la localización del usuario final, como por ejemplo:

* Bloquear/permitir contenido desde determinados países.
* Enviar las peticiones a un origen u otro en base a la localización geográfica del usuario final.
* Servir contenido especial por países.
* Reescribir cabeceras en base al país de procedencia de la petición.
* Reglas personalizadas en base a esa cabecera.

Para ver cómo implementar geobloqueos con esta cabecera puedes ir [aquí.](../../../security/bloqueando-peticiones-geograficamente.md)


# geo\_country\_code

De manera predeterminada, en Transparent Edge Services geolocalizamos todas las peticiones que pasan por nuestros sistemas, enviando a origen siempre una cabecera con el código del país desde el cual se hizo la petición. Esta cabecera es **geo\_country\_code**.

```
geo_country_code: ES
```

Si el sistema no ha sido capaz de ubicar la IP del usuario, envía el string 'Unknown' dentro de la cabecera.

Gracias a esta funcionalidad, somos capaces de hacer multitud de cosas en función de la localización del usuario final, como por ejemplo:

* Bloquear/permitir contenido desde determinados países.
* Enviar las peticiones a un origen u otro en base a la localización geográfica del usuario final.
* Servir contenido especial por países.
* Reescribir cabeceras en base al país de procedencia de la petición.
* Reglas personalizadas (_custom_) en base a esa cabecera.

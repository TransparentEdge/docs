# Geo\_Country\_Code

De manera predeterminada, en Transparent Edge geolocalizamos todas las peticiones que pasan por nuestros sistemas enviando a origen siempre una cabecera con el código del país desde el que se hizo la petición. Esta cabecera es **Geo\_Country\_Code**.

```
geo_country_code: ES
```

Si el sistema no ha sido capaz de ubicar la IP del usuario, envía el _string_ _Unknown_ dentro de la cabecera.

Gracias a esta funcionalidad, somos capaces de hacer multitud de cosas en función de la localización del usuario final, por ejemplo:

* Bloquear/permitir contenido desde determinados países.&#x20;
* Enviar las peticiones a un origen u otro en base a la localización geográfica del usuario final.&#x20;
* Servir contenido especial por países.&#x20;
* Reescribir cabeceras en base al país de procedencia de la petición.&#x20;
* Reglas personalizadas (_custom_) en base a esa cabecera.

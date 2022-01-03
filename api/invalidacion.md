---
description: >-
  A través del API ponemos a disposición de nuestros clientes un servicio de
  invalidación de recursos. A continuación se describen los detalles y las
  instrucciones de uso del mismo.
---

# Invalidación

El procedimiento para invalidar recursos contra el API es relativamente sencillo. Comenzaremos realizando una llamada a esta URL:

```bash
https://api.transparentcdn.com/v1/companies/{COMPANY_ID}/invalidate/
```

En esta petición, deberemos incluir una lista o array dentro de un objeto _JSON_ en el que indicaremos los recursos que queremos invalidar, identificados por sus **URLs**.&#x20;

```javascript
{
    "urls":[
            "http://www.servidor.com/recurso1",
            "http://fotos.servidor2.com/imagen2.jpg"
        ]
}
```

Podemos ejecutar esta llamada a través de [curl](https://www.mit.edu/afs.new/sipb/user/ssen/src/curl-7.11.1/docs/curl.html) o de cualquier interfaz que permita realizar llamadas HTTP. A continuación mostramos un ejemplo de cómo se compondría una llamada para invalidar los recursos _recurso1_ e _imagen2._

```bash
curl -XPOST -H 'Authorization: Bearer 58530ad8f1879786dbfaa3b9dad114c1dae1070280fdb' -d '{"urls":["http://www.servidor.com/recurso1","http://fotos.servidor2.com/imagen2.jpg"]}' -H "Content-Type: application/json" https://api.transparentcdn.com/v1/companies/42/invalidate/
```

Esta petición devolverá una respuesta en formato JSON, donde podremos encontrar las **URLs** que se han enviado para ser invalidadas. El resultado sería el siguiente, siempre y cuando _servidor.com_ y _fotos.servidor.com_ estuvieran dados de alta como _sites_ dentro de nuestro sistema y, además, estuviesen asociados al cliente que hace la petición.

```javascript
{
    "urls": [
        "http://fotos.servidor.com",
        "http://fotos.servidor.com/imagen2.jpg"
    ],
}
```

Nuestra API provee de un sistema para ejecutar invalidaciones recursivas. De forma que todas las **URLs** que coincidan con las que se envíen serán invalidadas. Para ello, habrá que incluir en la petición un parámetro adicional: "ban" con valor "1" para ejecutar la este servicio. Así, ejecutando la petición con el siguiente mensaje se marcará como contenido inválido todo lo que empiece por "_/recursos/_" de _fotos.servidor.com_.

```javascript
{
    "urls": [
        "http://fotos.servidor.com/recursos/"
    ],
    "ban": "1"
}
```


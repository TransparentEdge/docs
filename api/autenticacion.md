---
description: >-
  Instrucciones para la autenticación de usuarios contra la API de Transparent
  Edge Services.
---

# Autenticación

Para la autenticación de la API usamos el sistema de federación de OAuth2. Esto quiere decir que para hacer peticiones a la API, hace falta tener un token de acceso para la autenticación.

Antes de nada, vamos a necesitar los valores de `client_id` __ y `client_secret`se pueden obtener desde el panel: [https://dashboard.transparentcdn.com/user/edit](https://dashboard.transparentcdn.com/user/edit) pinchando tu nombre de usuario, después en "Opciones de Cuenta" y finalmente en "Gestión de Claves":

![](<../.gitbook/assets/image (55).png>)

Apunta los valores que aparecen, nos harán falta en los siguientes pasos:

![](<../.gitbook/assets/image (56).png>)

A continuación se da un ejemplo, con datos falsos, de como sería todo el proceso de obtención del token de acceso (API\_TOKEN).

> client\_id:0b58b22d51a2d68Ffdf17b\
> client\_secret: 0f3b38f9721211e848e39be374a4c1431386abdfe86\
> company\_id: 42
>
> Con estos datos, el primer paso es hacer una llamada a la API para obtener el token de autorización. Esto cambia según el lenguaje de programación usado, por lo que a continuación simplemente se da un ejemplo de llamada básico con CURL desde un shell bash.

```
curl -XPOST -d "client_id=0b58b22d51a2d68df17b&client_secret=0f3b38f9701e848e39be374a4c1431386abdfe86&grant_type=client_credentials" 
https://api.transparentcdn.com/v1/oauth2/access_token/
```

Esta petición devolverá un resultado, en formato JSON, similar a este:

```javascript
{
  "access_token": "58530ad8f1879786dbfad7Gh1a3b94c1dae1070280fdb",
  "token_type": "Bearer",
  "expires_in": 31535999,
  "refresh_token": "63553322177e88fHa8Kb25b122542e35733ce2a9cc24",
  "scope": "read write"
}
```

El campo access\_token es el que es básico para seguir haciendo peticiones a la API. Todas las peticiones que lleven una cabecera "Authorization: Bearer " ya irán autenticadas con el usuario que ha solicitado el access\_token. Por ejemplo una petición similar a la que se expone a continuación devolverá un objeto JSON con el usuario almacenado en nuestros sistemas.

```javascript
curl -v -H 'Authorization: Bearer 58530ad8f1879786dbfad7Gh1a3b94c1dae1070280fdb' https://api.transparentcdn.com/v1/companies/current_user/
```


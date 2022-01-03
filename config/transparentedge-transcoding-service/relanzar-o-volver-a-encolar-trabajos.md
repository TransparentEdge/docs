# Relanzar o volver a encolar trabajos

Podemos relanzar órdenes que hayan fallado mediante nuestra [API](https://api.transparentcdn.com/docs), lo que ayudaría a relanzar múltiples trabajos de forma programática. Para ello debemos estar [autenticados](https://docs.transparentedge.eu/api/autenticacion), lo cual es condición necesaria para poder hacer llamadas a nuestra API.

El endpoint que deberíamos utilizar sería el siguiente:

```
https://api.transparentcdn.com/media/{company_id}/transcode_requeue/{order_id}/
```

Ejemplo:

```
https://api.transparentcdn.com/v1/media/111/transcode_requeue/5596731515-3324
```

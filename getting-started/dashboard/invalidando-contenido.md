# Invalidando contenido

Para invalidar contenido, accede al apartado de invalidaciones a través del [panel](https://dashboard.transparentcdn.com/).

![](<../../.gitbook/assets/Captura de pantalla 2022-12-21 a las 18.02.38.png>)

Existen tres tipos de invalidaciones que detallaremos más adelante:

* Invalidaciones simples (PURGES)
* Invalidaciones suaves (SOFTPURGES)
* Invalidaciones recursivas (BANS)

Estos tres tipos de invalidación se solicitan mediante el botón "**PURGAR URLS**" y serán de un tipo u otro según las opciones seleccionadas.

Además, existe una opción extra: _refetching_ de caché o _warm up_ caché, disponible en los métodos de PURGE y SOFTPURGE, que calentará la caché del recurso purgado en todos los nodos de la plataforma tras su invalidación.

Para activar dicha opción, basta con acceder al menú "Opciones" antes de enviar la lista de invalidaciones y marcar la casilla que se muestra en la siguiente captura:

![](<../../.gitbook/assets/Captura de pantalla 2022-12-21 a las 18.11.30.png>)

{% hint style="info" %}
Para agregar _refetching_ a un PURGE ó SOFTPURGE al solicitar la invalidación mediante la [API](https://docs.transparentedge.eu/getting-started/faq/glosario/api), basta con agregar el campo "_refetch_" con el valor _true_, al _payload_ enviado en formato JSON:

`{..., "refetch": true}`
{% endhint %}

Por otro lado, en la parte inferior del apartado de invalidaciones se muestra un listado con las últimas realizadas:

![](<../../.gitbook/assets/Captura de pantalla 2022-12-21 a las 18.15.11.png>)

### Invalidaciones simples (PURGES)

{% hint style="info" %}
En la terminología de [_Varnish_](https://varnish-cache.org/), a este tipo de invalidaciones se las conoce como _PURGES_.
{% endhint %}

Estas invalidaciones utilizan la referencia absoluta al objeto a invalidar, que en este caso sería, para entendernos, la URL completa con su _query string_ si la tiene.

Por ejemplo, si ponemos en la caja de _tex_to "[http://www.example.com/style.css](http://www.example.com/style.css)", se lanzará una invalidación del objeto style.css, de manera que forzaremos a las cachés de Transparent Edge a ir a buscar a los servidores de origen una copia nueva de este objeto.

Si ese mismo objeto tiene distintas versiones almacenadas en caché ya que utiliza, por ejemplo, el _query string_: `version,` deberíamos invalidar la versión concreta del objeto: "[http://www.example.com/style.css?version=20210720](http://www.example.com/style.css?version=20210720)"

En el apartado "Opciones", para realizar una invalidación simple no necesitas marcar ninguna casilla a no ser que la quieras acompañar de un _refetch_.

{% hint style="success" %}
Este método es compatible con la opción de _refetching_ o _warm up_ caché.
{% endhint %}

### Invalidaciones suaves (SOFTPURGES)

En el _soft purge_ no purgamos directamente el objeto de la caché, sino que lo marcamos como caducado. Los objetos marcados como "caducados" (_stale_) se comportan como si su vida en la caché hubiera expirado. Por lo tanto, la próxima petición bajará a origen a buscar una nueva versión de este objeto.

La gran ventaja de este tipo de purgado es que, si en el momento de ir a buscar la nueva versión al _backend_ de origen este no responde (el _backend_ tiene algún problema), se servirá el objeto _stale_ en lugar de presentar un error al cliente. La gran mayoría de las veces es mejor presentar un objeto "caducado" que un error.

Este tipo de invalidación se solicita exactamente igual que el tipo "PURGE", con la diferencia de que en el apartado de "Opciones" marcaremos la casilla "Purgado suave".

![](<../../.gitbook/assets/Captura de pantalla 2022-12-21 a las 18.22.22.png>)

{% hint style="info" %}
Para transformar un PURGE en SOFTPURGE al solicitar la invalidación mediante la [API](https://docs.transparentedge.eu/getting-started/faq/glosario/api), basta con agregar el campo "_soft_" con el valor _true_, al _payload_ enviado en formato JSON:

`{..., "soft": true}`
{% endhint %}

{% hint style="success" %}
Este método es compatible con la opción de _refetching_ o _warm up_ caché.
{% endhint %}

### Invalidaciones recursivas (BANS)

Del mismo modo que puedes invalidar una URL de manera absoluta (con sus parámetros), hay ocasiones en las que necesitas una invalidacion de contenidos que se lleve a cabo de forma recursiva.

Para ello, introduce el patrón de la URL que quieras invalidar en el campo de texto, por ejemplo. `https://www.transparentcdn.com/images/`, y en el apartado "Opciones" marca la casilla "Quiero banear todos los activos que coinciden con la expresión".

![](<../../.gitbook/assets/Captura de pantalla 2022-12-21 a las 18.25.01.png>)

Si lo marcas y pulsas el botón "PURGAR URLS", se lanzará una invalidación del contenido de forma recursiva que hará que se invalide todo lo que coincida con `https://www.transparentcdn.com/images/`.

{% hint style="warning" %}
Este método NO es compatible con la opción de _refetching_ o _warm up_ caché.
{% endhint %}

{% hint style="danger" %}
Este tipo de invalidados, en según qué casos, puede resultar muy peligroso. Si invalidas, por ejemplo, / (https://www.transparentcdn.com/) de forma recursiva, puedes tener serios problemas en tu plataforma de origen, ya que de pronto tendrá un aluvión de tráfico procedente de todos tus usuarios. Úsalo con cabeza.
{% endhint %}

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com) puedes hacerlo desde nuestro [API](../faq/glosario/api.md).
{% endhint %}

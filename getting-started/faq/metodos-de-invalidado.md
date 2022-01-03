# Métodos de invalidado

Disponemos de dos métodos para invalidar contenido: lo que llamamos **PURGE,** que son invalidaciones sencillas en las que invalidas un objeto en particular, y las de tipo **BAN**, que son invalidaciones recursivas que nos permiten invalidar más de un objeto a la vez.

### Cache Key

La _cache key_ o clave de caché es la forma que tiene la CDN de guardar un objeto en su caché. De manera predeterminada, la _cache key_ está formada por:

```
[Host header] + [url] + [Query Stream]
```

A partir de ahí, podemos complicar la _cache key_ todo lo que queramos, añadiendo más elementos para guardar más versiones del mismo objeto. Veámoslo con un ejemplo:

```
https://www.transparentedge.eu/index.html?param=1

https://www.transparentedge.eu/index.html?param=2
```

Para Transparent Edge Services y, en general, para cualquier sistema de cachés http, estos son dos objetos distintos. Por tanto, aunque el primero esté cacheado, cuando se solicite el segundo, volverá a bajar a origen.

Mediante la cabecera [Vary](cabeceras-por-defecto/vary.md) podemos modificar el comportamiento y guardar más versiones del mismo objeto. Por ejemplo, dado un objeto como este

```
https://www.transparentedge.eu/index.html?param=1
```

y una cabecera Vary como esta

```
Vary: User-Agent
```

podemos hacer que en la caché de la CDN se guarden tantos objetos como User-Agent lo soliciten.&#x20;

{% hint style="warning" %}
Este ejemplo en particular hace que la eficiencia de la caché baje drásticamente,  por lo que no es recomendable tener un Vary por User-Agent o por Cookie. Podría ser interesante, por ejemplo, tenerlo por la cabecera [X-Device](cabeceras-por-defecto/x-device.md), de manera que puedas servir diferentes versiones del objeto en función del tipo de dispositivo que solicita el recurso.
{% endhint %}

### Invalidaciones simples o de tipo PURGE

Este tipo de invalidaciones nos permiten invalidar un contenido en particular de la caché. Para ello deberás asegurarte de meter la url completa con parámetros incluidos.

{% hint style="info" %}
Si has modificado el comportamiento de la _cache key_ con la cabecera Vary, en principio no deberías preocuparte. Una invalidación de este tipo debería invalidar todas las versiones que existan de ese objeto en particular.
{% endhint %}

Dentro de las invalidaciones de tipo PURGE existe una forma de invalidado suave que se conoce como SOFTPURGE, la cual realiza la invalidación de una forma menos agresiva.&#x20;

Para llevar a cabo una invalidación con desde nuestro __ [_dashboard_](https://dashboard.transparentcdn.com) puedes seguir esta [guía](../dashboard/invalidando-contenido.md).

### Invalidaciones recursivas o de tipo BAN

Las invalidaciones de tipo BAN permiten invalidar contenido de la caché de forma recursiva, enviando un patrón e indicándole a la caché que invalide cualquier objeto que coincida con dicho patrón. Por ejemplo, podemos mandar a invalidar la url **https://www,transparentedge.eu/images/.** Esto invalidará cualquier objeto que coincida con esta url por ejemplo **https://www,transparentedge.eu/images/imagen1.png** o **https://www,.ransparentedge.eu/images/2020/02/22/index.php.**

Para llevar a cabo una invalidación desde nuestro [_dashboard_](https://dashboard.transparentcdn.com) puedes seguir esta [guía](../dashboard/invalidando-contenido.md).

{% hint style="warning" %}
Este tipo de invalidaciones son incompatibles con la funcionalidad de _refecthing_.
{% endhint %}

{% hint style="danger" %}
Este tipo de invalidados, en según que casos, puede resultar muy peligroso. Por ejemplo, si invalidas / (https://www.transparentedge.eu/) de forma recursiva, puedes tener serios problemas en tu plataforma de origen, ya que tendrá de pronto un aluvión de tráfico procedente de todos tus usuarios. Úsalo con cabeza.
{% endhint %}



{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com) __ puedes hacerlo desde nuestro [API](glosario/api.md).
{% endhint %}

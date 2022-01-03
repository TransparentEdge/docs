# Cache key

La _cache key_ o clave de caché es la forma que tiene la CDN de guardar un objeto en su caché. De manera predeterminada, la _cache key_ está formada por:

```
[Host header] + [url] + [Query Stream]
```

A partir de ahí podemos complicar la _cache key_ todo lo que queramos, añadiendo más elementos para guardar más versiones del mismo objeto. Veámoslo con un ejemplo:

```
https://www.transparentedge.eu/index.html?param=1

https://www.transparentedge.eu/index.html?param=2
```

Para Transparent Edge Services y, en general, para cualquier sistemas de cachés http, estos son objetos distintos y, por tanto, aunque el primero este cacheado, cuando se solicite el segundo, volverá a bajar a origen. Es decir, en la caché de la CDN son dos objetos distintos.

Mediante la cabecera [Vary](../cabeceras-por-defecto/vary.md) podemos modificar el comportamiento y guardar más versiones del mismo objeto, tal y como deciamos anteriormente. Por ejemplo, dado un objeto como este

```
https://www.transparentedge.eu/index.html?param=1
```

y una cabecera Vary como esta

```
Vary: User-Agent
```

podemos hacer que en la caché de la CDN se guarden tantos objetos como el número de User-Agent que lo solicitan.&#x20;

{% hint style="warning" %}
En el ejemplo anterior, la caché nos guardaría un objeto por cada User-Agent distinto (existen muchísimos). Esto haría que el sistema de cachés no fuera nada efectivo. Por lo tanto, no es recomendable tener un Vary por User-Agent o por Cookie. Podría ser interesante, por ejemplo, tenerlo por la cabecera [X-Device](../cabeceras-por-defecto/x-device.md), de manera que puedas servir diferentes versiones del objeto en función del tipo de dispositivo que solicita el recurso.
{% endhint %}

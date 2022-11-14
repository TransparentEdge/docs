# Cache key

Una c_ache key_ o clave de caché es la forma que tiene la CDN de guardar un objeto en su caché. De manera predeterminada, la _cache key_ está formada por:

```
[Host header] + [url] + [Query Stream]
```

A partir de ahí podemos complicar la _cache key_ todo lo que queramos, añadiendo más elementos para guardar más versiones del mismo objeto. Veámoslo con un ejemplo:

```
https://www.transparentedge.eu/index.html?param=1

https://www.transparentedge.eu/index.html?param=2
```

Para Transparent Edge y, en general, para cualquier otro sistema de cachés HTTP, estos son objetos distintos. Por lo tanto, aunque el primero esté cacheado, volverá a bajar a origen cuando se solicite el segundo ya que en la caché de la CDN son dos objetos distintos.

Mediante la cabecera [Vary](../cabeceras-por-defecto/vary.md) podemos modificar el comportamiento y guardar más versiones del mismo objeto, tal y como decíamos anteriormente. Por ejemplo, dado un objeto como este:

```
https://www.transparentedge.eu/index.html?param=1
```

Y una cabecera Vary como esta

```
Vary: User-Agent
```

Podemos hacer que en la caché de la CDN se guarden tantos objetos como el número de _user-agent_ (agente de usuario) que lo solicita.

En el ejemplo anterior, la caché nos guardaría un objeto por cada _user-agent_ distinto (existen muchísimos). Esto haría que el sistema de cachés no fuera efectivo. Por lo tanto, no es recomendable tener una cabecera _Vary_ por _user-agent_ o por _cookie_. Podría ser interesante, sin embargo, tenerlo por la cabecera _X-Device_, de manera que puedas servir diferentes versiones del objeto en función del tipo de dispositivo que solicita el recurso.

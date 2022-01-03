# Cabeceras de caché

Las cabeceras de caché son clave para el correcto funcionamiento de un sistema de cachés en general y de Transparent Edge Services en particular.\
Conocer el uso de las cabeceras de caché va a aumentar significativamente la velocidad de carga de una página web y, por tanto, mejorará la experiencia de usuario.

Hay varios tipos de cabecera mediante los cuales Transparent Edge Services entiende que debe o no guardar ese objeto en su caché, pero en este documento vamos a centrarnos en la cabecera **Cache-Control**.

En Transparent Edge Services, la cabecera Cache-Control tiene preferencia sobre todas las demás cabeceras de caché, por lo que en caso de encontrarse con varias, será a esta a la que haremos caso.

La cabecera **Cache-Control** se introduce como mejora en HTTP/1.1 para simplificar los mecanismos de caché que había hasta el momento en el protocolo HTTP/1.0. Con esta cabecera indicamos tanto a las cachés intermedias como a la caché del navegador si un objeto debe o no guardarse y por cuánto tiempo.\
Vamos al grano: la cabecera **Cache-Control,** como todas las cabeceras en HTTP, es un par **clave-valor,** siendo la clave siempre la palabra Cache-Control. Desde el punto de vista de respuesta del servidor, el valor puede tomar los siguientes estados:

#### **public**

La opción **public** se usa para indicar a la caché que este objeto es público y por tanto cacheable. No suele encontrarse sola en el Cache-Control.

#### **private**

A diferencia de public, la cabecera **private** indica que este contenido no debe cachearse, por lo general porque es contenido de usuario personalizado y debe, por la lógica de la aplicación, bajar hasta los servidores de origen del cliente.

#### **no-cache**

La cabecera **no-cache** evita que un objeto se cachee en Transparent Edge Services. Aunque sí se almacena, nunca se usa.

#### **no-store**

Está opción impide a la caché que ese objeto sea almacenado y, por tanto, cacheado.

{% hint style="info" %}
Tanto _private_ como _no-cache_ y _no-store_, aunque con matices, tienen el mismo efecto en una navegación y es que el contenido va a bajar hasta los servidores de origen del cliente.
{% endhint %}

#### **must-revalidate**

Esta opción fuerza a la caché a que vaya a por una copia nueva del objeto a origen, por lo que debería obviar el resto de cabeceras de caché e ir a buscar una copia nueva.

#### **proxy-revalidate**

Funciona igual que **must-revalidate** pero solo afecta a los _proxys_.

#### **max-age**

Con **max-age** indicamos a Transparent Edge Services durante cuánto tiempo en segundos queremos guardar ese objeto en la caché y, por tanto, marcarlo como válido. Una vez ese tiempo llegue a su fin, el objeto se marca como caducado y se fuerza a ir a los servidores de origen del cliente a por una copia nueva. Esta opción afecta tanto a _proxys_ como a cachés de navegadores.

#### **s-maxage**

**s-maxage** es exactamente igual que max-age, con la salvedad de que solo afecta al comportamiento de los _proxys_ y no de las cachés de los navegadores. Se pueden usar en conjunción para lograr comportamientos especiales como, por ejemplo:

```
Cache-Control: max-age=0, s-maxage=3600
```

En este caso especial, el objeto se guardará en Transparent Edge Services durante una hora (3600s), mientras que en el navegador no se guardará.

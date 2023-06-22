# Cabeceras de caché

Las cabeceras de caché son clave para el correcto funcionamiento de un sistema de cachés en general y de Transparent Edge en particular. Conocer su uso aumentará significativamente la velocidad de carga de una página web y, por tanto, mejorará la experiencia de usuario.

Hay varios tipos de cabecera mediante los cuales Transparent Edge entiende que debe o no guardar un objeto en su caché, pero en este documento vamos a centrarnos en la cabecera _**Cache-Control**_.

En Transparent Edge, la cabecera Cache-Control tiene preferencia sobre todas las demás cabeceras de caché, por lo que en caso de encontrarse con varias, será a esta a la que haremos caso.

La cabecera **Cache-Control** es una mejora en HTTP/1.1 que simplifica los mecanismos de caché que había hasta el momento en el protocolo HTTP/1.0. Con esta cabecera indicamos tanto a las cachés intermedias como a la caché del navegador si un objeto debe o no guardarse y por cuánto tiempo.

Como todas las cabeceras en HTTP, _**Cache-Control**_ es un par **clave-valor,** siendo la clave siempre la palabra _Cache-Control_. Desde el punto de vista de respuesta del servidor, el valor puede tomar los siguientes estados:

#### **Public**

La opción _**Public**_ se usa para indicar a la caché que este objeto es público y, por tanto, cacheable. No suele encontrarse sola en el P.

#### **Private**

A diferencia de _Public_, la cabecera _**Private**_ indica que este contenido no debe cachearse porque es contenido de usuario personalizado y debe, por la lógica de la aplicación, bajar hasta los servidores de origen del cliente.

#### **No-Cache**

La cabecera _**No-Cache**_ evita que un objeto se cachee en Transparent Edge. Aunque sí se almacena, nunca se usa.

#### **No-Store**

Está opción impide a la caché que ese objeto sea almacenado y, por tanto, cacheado.

{% hint style="info" %}
Tanto _Private_ como _No-Cache_ y _No-Store_, aunque con matices, tienen el mismo efecto en una navegación:  hacen que el contenido baje hasta los servidores de origen del cliente.
{% endhint %}

#### **Must-Revalidate**

Esta opción fuerza a la caché a que vaya a por una copia nueva del objeto a origen, por lo que debería obviar el resto de cabeceras de caché e ir a buscar una copia nueva.

#### **Proxy-Revalidate**

Funciona igual que _**Must-Revalidate**_ pero solo afecta a los _proxies_.

#### **Max-Age**

Con _**Max-Age**_ indicamos a Transparent Edge durante cuánto tiempo en segundos queremos guardar ese objeto en la caché y, por tanto, marcarlo como válido. Una vez ese tiempo llegue a su fin, el objeto se marca como caducado y se fuerza a ir a los servidores de origen del cliente a por una copia nueva. Esta opción afecta tanto a _proxies_ como a cachés de navegadores.

#### **S-Maxage**

_**S-Maxage**_ es exactamente igual que _Max-Age_, con la salvedad de que solo afecta al comportamiento de los _proxies_ y no de las cachés de los navegadores. Se pueden usar en conjunción para lograr comportamientos especiales, por ejemplo:

```
Cache-Control: max-age=0, s-maxage=3600
```

En este caso especial, el objeto se guardará en Transparent Edge durante una hora (3600s), pero no se guardará en el navegador.

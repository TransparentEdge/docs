# Cosas a tener en cuenta

Hay varias cosas a tener en cuenta cuando trabajas con una CDN por delante de tu servicio web. Estas son las más importantes:

### Actualización del contenido

Cuando trabajas con una CDN, debes ser consciente de que tienes un elemento delante de tu sitio web que está cacheando el contenido que publicas. Si quieres actualizar cualquier elemento de la página, es importante que te asegures de que, una vez esté a tu gusto, ese contenido cacheado se elimine de la caché de la CDN para dar paso al contenido nuevo.

Para eliminar ese contenido, proceso que se conoce como invalidar contenido, tienes varias opciones.

La primera y la más sencilla es que, después de una actualización de contenido, vayas a nuestro [portal](https://dashboard.transparentcdn.com) a la sección de **Invalidaciones** y, pulsando el botón _Purge URLs_, introduzcas las URL a invalidar.

![](<../../.gitbook/assets/Captura de pantalla 2020-05-21 a las 9.59.08.png>)

Otra opción es que integres vía API el proceso de invalidación dentro del sistema de publicación, de manera que tu sistema lo detecte automáticamente cuando cambies un contenido y lance una invalidación a través de nuestro [API](https://api.transparentcdn.com/docs).&#x20;

Si estás usando [WordPress](https://docs.transparentcdn.com/integraciones/configuracion-plugin), [Magento](../../integraciones/plugin-para-magento.md) o [Prestashop](../../integraciones/plugin-para-prestashop.md), tenemos _plugins_ a tu disposición que se encargan precisamente de integrar el CMS con Transparent Edge para que no tengas que preocuparte de nada.

### **IP del cliente**

Cuando estás detrás de nuestra CDN no te va llegar la dirección IP de cliente de la manera habitual, ya que es Transparent Edge la que hace las peticiones en nombre del cliente a los servidores de origen. Pero, ¡tranquilidad!, no está todo perdido.

Para lidiar con esta situación, lo que hacemos en Transparent Edge es crear cabeceras HTTP extra que nos permiten pasar al cliente la dirección IP del usuario real que está navegando. Así, aunque el servidor esté detrás de la red de caché, el cliente puede tomar las medidas oportunas en función de la IP real. Esto lo realizamos con las cabeceras _X-Forwarded-For_ y/o _True-Client-Ip_.

Te lo explicamos con detalle [aquí](cabeceras-por-defecto/true-client-ip-y-x-forwarded-for.md).

### **Sistemas IPS/IDS**

Si en tu plataforma de origen cuentas con sistemas IDS/IPS, debes extremar las precauciones, ya que estos sistemas, aunque están pensados para proteger tu plataforma, pueden causar problemas cuando trabajas con una CDN por delante.&#x20;

Una de las muchas bondades de los IPS es que son capaces de detectar cuándo desde una IP en concreto se está accediendo a un recurso de manera no lícita.&#x20;

Como comentábamos anteriormente, cuando contamos con una CDN por delante, las peticiones que se hagan a tu servidor de origen serán todas desde IP pertenecientes a la CDN. Por eso puede darse el caso de que el IPS piense que las IP de la CDN están haciendo más peticiones de lo que debería una IP y sean baneadas como si de un ataque se tratara.&#x20;

Obviamente, nos encontramos ante un falso positivo que se puede subsanar, pero es algo a tener muy en cuenta.

En Transparent Edge publicamos va API nuestros rangos de [IP](https://api.transparentcdn.com/v1/companies/ipranges/), por lo que la forma recomendada de actuar cuando cuentas con este tipo de sistemas es la de incluir todas las IP de Transparent Edge en una _whitelist_ dentro del IPS.

### **Dominios raíz**

En Transparent Edge, como en muchas otras CDN, la manera más común de poner a funcionar el servicio es haciendo un sencillo cambio a nivel DNS de forma que, por ejemplo, el subdominio www de tu _site_ deje de ser un registro de tipo A y pase a ser un registro de tipo CNAME que nosotros te daremos con el alta del servicio.

Hasta aquí, todo bien. Pero si tu sitio web funciona directamente con el dominio canónico, es decir, sin el www o cualquier otro dominio (p.e: **https://misite.com** en lugar del **htttps://www.misite.com**), vas a tener que hacer algún trabajo extra:

* No todos los proveedores de DNS soportan esta opción, pero si tu dominio no tiene registros MX asociados, en teoría si podrías hacer el CNAME del dominio canónico. Repetimos: esta opción, pese a ser la más fácil, no está disponible en todos los proveedores de DNS.
* Otra opción sería delegarnos la gestión del dominio dentro de nuestro sistema DNS.

### _**Set-Cookies**_** y páginas con autenticación básica**

Las páginas web que lleven en la respuesta la cabecera _Set-Cookies_ o _Authorization_ no serán cacheadas por defecto en Transparent Edge.

###


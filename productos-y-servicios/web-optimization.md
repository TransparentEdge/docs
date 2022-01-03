# Edge Distributed Content

El principal reto en el diseño de una página web es crear una aplicación funcional que también sea rápida y segura. Edge Distributed Content, además de acercar el contenido al usuario final, utiliza los últimos algoritmos de cacheo y hace que las páginas carguen hasta 10 veces más rápido. Es la solución perfecta para distribuir contenido por internet de una manera económica, rápida y segura.

**Dynamic & Static Caching**

Cacheamos objetos estáticos y dinámicos, y así se reduce notablemente el tiempo de carga de todas las páginas.

* **Compression:** Se envía el contenido comprimido desde la capa de caché hasta el usuario final. De esta manera se mejora el rendimiento y se reducen los tiempos de carga de las páginas, al mismo tiempo que se ahorra ancho de banda**.**
* **Header rewrite:** Se controlan las cabeceras de aplicación HTTP que llegan a nuestros sistemas con posibilidad de ser reescritas si fuera necesario.
* **HTTP2/HTTPS:** Sin coste adicional para nuestros clientes, ciframos la conexión entre usuario y servidor securizando su contenido y, además, mejoramos la entrega del mismo.

**Fast Purging**

Nuestro sistema de purgado garantiza que el contenido es purgado de nuestros nodos en un tiempo inferior a cinco segundos.

* **Purge:** Purgado de urls absolutas.
* **Ban:** Purgado de urls de forma recursiva.
* **Surrogate Keys:** Purgado de objetos mediante etiquetas.

**Edge Computing**&#x20;

En Transparent Edge Services te damos la oportunidad de acercar al usuario final parte de la lógica de aplicación, lo que descarga al servidor de tareas que pueden ser ejecutadas de manera descentralizada y de forma más óptima.

* **Mobile Detection:** Detectamos en cada petición el dispositivo desde el que se conecta el usuario y entregamos el contenido acorde al mismo.
* **Location Detection:** Detectamos la ubicación del usuario y marcamos el _country code_ en cada _request_ para facilitar los procesos de negocio.
* **Geoblocking:** Bloqueo o redirección de tráfico por países**.**
* **Paywall:** Permitimos que el cliente monetize todos sus contenidos mediante muros de pago.
* **ESI:** Edge Side Include de forma nativa te permitirá componer tu página por fragmentos en el _edge_.

**Smart Architecture & Security**&#x20;

Nuestra arquitectura está diseñada para optimizar al máximo el rendimiento de los sitios web del cliente.

* **43 PoP:** Tenemos 43 puntos de presencia repartidos en 26 países.
* **Midtier:** Nuestra arquitectura de dos capas reduce en gran medida el número de peticiones que llegan a los servidores de origen de los clientes.
* **Transparent WAF:** Integrado con nuestro servicio de entrega de contenidos te permitirá mantener a salvo tus sitios web de los principales ataques conocidos.
* **AntiDos:** Gracias a nuestros sistemas somos capaces de mitigar ataques de hasta 1Tbps.

****[**API & Web Interface**](../getting-started/dashboard/)****

Transversal a todos nuestros productos, nuestra API REST te permite interactuar con nuestra plataforma en tiempo real e integrar el servicio con herramientas de terceros**.**

* **Histórico de tráfico:** Todo el tráfico consolidado desde el inicio del servicio.
* **Análitica en tiempo real:** Explota los datos de tus sitios web en tiempo real en nuestro sistema de analítica Big Data.
* **Purging:** Integra nuestra API en tus sistema para automatizar los engorrosos procesos de invalidación de contenido.
* **Mixed Content Detection:** Detectamos las peticiones bloqueadas por el navegador de los usuarios al intentar cargar contenido http dentro de una _site_ https y lo registramos para facilitar la tarea de detección de ese contenido bloqueado.
* **Trending News:** Te mostramos un listado en tiempo real con las noticias más visitadas de tu sitio web.
* **Caché warming:** Arquitectura de CDN moderna. Los sistemas de precalentamiento de caches son básicos para que la optimización y entrega del contenido a los usuarios sea mejor.
* **Refetching:** Tras la invalidación de cualquier tipo de contenido, ese mismo objeto se vuelve a solicitar al _edge_ de manera que esté fresco para el siguiente usuario que lo solicite.
* **Prefetching:** Te damos la posibilidad de enviar mediante API un listado de urls para realizar un precalentamiento de cachés.

![](<../.gitbook/assets/Captura de pantalla 2021-12-20 a las 18.06.20.png>)

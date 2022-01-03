---
description: Fundamentals
---

# Empecemos por el principio

## ¿Qué es una caché web?

Una caché web no es ni más ni menos que un sistema que nos permite almacenar objetos HTTP de manera dinámica.&#x20;

Está cache web puede encontrarse bien en el lado de cliente (navegadores web) o del lado del servidor (_proxys_ intermedios o servidores web).&#x20;

Este tipo de sistemas ofrecen una mejora significativa del rendimiento en la carga de objetos HTTP mejorando en ultima instancia los tiempos de carga de las paginas web y, por tanto, mejorando la experiencia del usuario final.

Resumiendo, es un sistema que guarda copias locales de los objetos web que los navegadores solicitan de manera que, cuando un navegador pide un objeto que está en la caché, este se sirve directamente desde él sin tener que ir al servidor de destino a generar de nuevo el objeto con todo el tiempo que ello implica.

## Evangelizando un poco, ¿Qué es una CDN?

Una CDN, es una red de entrega de contenidos (de sus iniciales en inglés Content Delivery Network). Simplificando mucho su funcionamiento una CDN es un sistema de caches distribuido geográficamente, de manera que en su concepto más básico es capaz de servir contenido al usuario final desde el servidor de caché más cercano a este, consiguiendo de esta forma reducir latencias de red y tiempos de carga de la página.

Mucha gente cree que una CDN se utiliza solo en el caso de reducir latencias de red para "acercar" su contenido al usuario final, pero la realidad no es así. El verdadero potencial de una CDN y lo que realmente te permite mejorar los tiempos de carga más allá de las laténcias de red, son las técnicas de caching, ya que servir un objeto cacheado desde memoria es infinitamente más rápido que generar una página web desde cero incluyendo las llamadas a su respectiva base de datos.

```bash
# Request goes to origin server 
curl  --tlsv1.3 -s -o /dev/null -w "%{time_total}\n" -H "User-Agent: Chrome" https://www.transparentcdn.com/?p=random
0,596708

# Request is served from CDN server
curl  --tlsv1.3 -s -o /dev/null -w "%{time_total}\n" -H "User-Agent: Chrome" https://www.transparentcdn.com/
0,167391
```

Como puede ver en el ejemplo el tiempo la mejora es muy significativa, en este caso 3,6 veces más rápido servir el contenido desde la CDN.

### Otras ventajas de una CDN

Adicionalmente a lo comentado en el punto anterior, una CDN tiene muchas otras ventajas como por ejemplo:

* **Optimización de los recursos en la plataforma de origen.** Puesto que la mayoría del tráfico es cacheado y servido desde la CDN, la plataforma de origen, en la cual el cliente alberga sus páginas web, ve como de manera repentina su tráfico se reduce en un 80-90%, lo que lleva a una reducción del número de recursos, ya sean servidores o ancho de banda o, incluso, de personal dedicado al mantenimientos de los mismos.
* **Ahorro de costes.** Debido a la optimización de recursos explicada en el punto anterior, una  CDN consigue un ahorro de costes en infraestructura que permite amortizar con creces el coste del servicio.
* **Aumento de la seguridad de los sites.** El carácter distribuido de una CDN permiten ocultar a los ojos del usuario final los servidores de origen, mejorando la seguridad. Adicionalmente servicios con Transparent Edge Services cuenta con servicios de seguridad adicionales como [Transparent Secure Layer](https://www.transparentcdn.com/servicios-empresa/#secure\_layer)
* **Adsorción de grandes picos y volúmenes de tráfico.** Con una  CDN es posible asumir grandes volúmenes de tráfico y hacer frente a picos que si fueran contra la plataforma de origen, seria muy difíciles de gestionar.
* **Fácil puesta en funcionamiento.** Normalmente poner a funcionar una CDN es tan sencillo como un simple cambio en el DNS del cliente de manera que el sitio web que se quiera pasar por la CDN deje de apuntar al servidor web de origen y empiece a apuntar al CNAME que la CDN provee para dar el servicio.

![](../../.gitbook/assets/Red\_Tcdn.png)

## Transparent Rocks!

Transparent Edge Services es la primera red contenidos de capital 100% español y con infraestructura en más de 30 países. Basada en [Varnish Enterprise](https://www.varnish-software.com) somos la única CDN del mundo que ofrece esta modalidad de Varnish como servicio con toda la potencia que esto conlleva. Además somos [Open Partner](https://www.varnish-software.com/partners/) de Varnish lo que nos da una garantía adicional a nuestro producto y servicio.

![](../../.gitbook/assets/Varnish-Software\_2.0\_POS\_Large.png)

Nacimos con la idea de hacer la CDN que a nosotros nos hubiera gustado tener como clientes y como tal nuestro primer activo es el capital humano y la cercanía nuestro principal valor.&#x20;

El objetivo de este site no es vender si no facilitar la vida a nuestros clientes, por tanto si estás leyendo esto, ya sabes quienes somos y probablemente conozcas nuestras bondades, es más, es posible que seas cliente, en cualquier caso, en nuestra [web](https://www.transparentedge.eu) tienes todo lo que necesitas saber a nivel comercial de nosotros y para todo lo demás siempre puedes enviar un correo a [equipo de ventas](mailto:sales@transparentedge.eu).

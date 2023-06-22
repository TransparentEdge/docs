---
description: Fundamentals
---

# Empecemos por el principio

## ¿Qué es una caché web?

Una caché web no es ni más ni menos que un sistema que nos permite almacenar objetos HTTP de manera dinámica. Está caché web puede encontrarse en el lado de cliente (navegadores web) o en el del servidor (_proxies_ intermedios o servidores web).&#x20;

Este tipo de sistemas ofrece una mejora significativa del rendimiento en la carga de objetos HTTP, reduciendo los tiempos de carga de las páginas web y, por tanto, optimizando la experiencia del usuario final.&#x20;

El sistema guarda copias locales de los objetos web que los navegadores solicitan y, cuando un navegador pide un objeto que está en la caché, este se sirve directamente desde él sin tener que ir al servidor de destino a generar de nuevo el objeto, con todo el tiempo que implica.

## ¿Qué es una CDN?

Una CDN es una red de entrega de contenidos (_content delivery network_ en inglés).&#x20;

Simplificando mucho su funcionamiento, una CDN es un sistema de cachés distribuido geográficamente, de modo que puede servir contenido al usuario final desde el servidor caché más cercano a este. Así reduce latencias de red y tiempos de carga de la página.&#x20;

Mucha gente cree que las CDN solo se utilizan para reducir latencias de red y acercar el contenido al usuario final, pero no es así. El verdadero potencial de una CDN y lo que realmente permite mejorar los tiempos de carga más allá de las latencias de red son las técnicas de _caching_: servir un objeto cacheado desde memoria es infinitamente más rápido que generar una página web desde cero incluyendo las llamadas a su respectiva base de datos.

```bash
# Request goes to origin server 
curl  --tlsv1.3 -s -o /dev/null -w "%{time_total}\n" -H "User-Agent: Chrome" https://www.transparentcdn.com/?p=random
0,596708

# Request is served from CDN server
curl  --tlsv1.3 -s -o /dev/null -w "%{time_total}\n" -H "User-Agent: Chrome" https://www.transparentcdn.com/
0,167391
```

Como puede verse en el ejemplo, la mejora en los tiempos es muy significativa. En este caso, resulta 3,6 veces más rápido servir el contenido desde la CDN.

### Otras ventajas de una CDN

Adicionalmente a lo comentado en el punto anterior, una CDN tiene muchas otras ventajas, entre ellas:

* **Optimización de los recursos en la plataforma de origen.** Puesto que la mayoría del tráfico es cacheado y servido desde la CDN, la plataforma de origen donde el cliente alberga sus páginas web ve cómo su tráfico se reduce en un 80-90%, lo que disminuye también el número de recursos necesarios, ya sean servidores, ancho de banda o, incluso, personal dedicado al mantenimientos de los mismos.
* **Ahorro de costes.** Debido a la optimización de recursos explicada en el punto anterior, una  CDN consigue un ahorro de costes en infraestructura que permite amortizar con creces el coste del servicio.
* **Aumento de la seguridad de los **_**sites**_**.** El carácter distribuido de una CDN permite ocultar a los ojos del usuario final los servidores de origen, mejorando la seguridad. Transparent Edge cuenta con servicios de seguridad adicionales como[Transparent Secure Layer](https://www.transparentcdn.com/servicios-empresa/#secure\_layer).
* **Absorción de grandes picos y volúmenes de tráfico.** Con una CDN es posible asumir grandes volúmenes de tráfico y hacer frente a picos que, si fueran contra la plataforma de origen, serían muy difíciles de gestionar.
* **Fácil puesta en funcionamiento.** Poner a funcionar una CDN es tan sencillo como realizar un simple cambio en el DNS del cliente, de manera que el sitio web que se quiera pasar por la CDN deje de apuntar al servidor web de origen y empiece a apuntar al CNAME que la CDN provee para dar el servicio.

<figure><img src="../../.gitbook/assets/Captura de pantalla 2022-11-14 a las 19.54.08.png" alt=""><figcaption></figcaption></figure>

## Transparent Edge Rocks!

Transparent Edge es la primera y única red de entrega de contenidos española y cuenta con infraestructura en más de 30 países. Basada en [Varnish Enterprise](https://www.varnish-software.com/) es la única CDN del mundo que ofrece esta modalidad de Varnish como servicio con toda la potencia que conlleva. Además, somos [Open Partner](https://www.varnish-software.com/partners/) de Varnish lo que da una garantía adicional a nuestro producto.

![](../../.gitbook/assets/Varnish-Software\_2.0\_POS\_Large.png)

El objetivo de este _site_ no es vender, sino facilitar la vida a nuestros clientes. Por eso, si estás leyendo esto, ya sabes quiénes somos y, probablemente, conozcas nuestras bondades. Es más, es posible que seas cliente ya. En cualquier caso, en nuestra [web](https://www.transparentedge.eu/) tienes todo lo que necesitas saber a nivel comercial de nosotros y para todo lo demás siempre puedes enviar un correo al [equipo de ventas](mailto:sales@transparentedge.eu).

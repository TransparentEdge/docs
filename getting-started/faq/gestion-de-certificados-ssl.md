# SSL

Transparent Edge Services puede hacer de terminador SSL para los _sites_ que lo requieran, cacheando las peticiones de igual manera que si se tratara de HTTP. En este caso, el tráfico viajaría cifrado entre usuario y Transparent Edge Services. En nuestras caches se descifraría el certificado y todo el tráfico dentro la plataforma de Transparent Edge Services viajaría en HTTP, permitiéndonos así la posibilidad de aplicar tecnologías de _web caching_ también al tráfico SSL. El tráfico entre Transparent Edge Services y el servidor de origen del cliente puede ir tanto en HTTP como HTTPS a demanda del cliente.

Por defecto damos HTTP2 sobre tlsv1.2 y tlsv1.3.



## Autogestión de certificados SSL

En Transparent Edge Services tendrás siempre varias alternativas para hacer una misma cosa. En este caso, te ofrecemos dos posibilidades para poder servir tu sitio web de manera segura.&#x20;

La primera es que compres tus certificados a tu proveedor de confianza y, una vez los tengas, nos los subas a nuestra plataforma (o nos mandes un [ticket](mailto:support@transparentcdn.com) para que podamos hacerlo por ti).

Para hacer esto, debes ir a nuestro [portal de cliente](https://dashboard.transparentcdn.com), seleccionar en el menú de la izquierda **Autoprovisioning** y luego hacer clic en **Custom Certs.**&#x20;

![](<../../.gitbook/assets/Captura de pantalla 2020-05-21 a las 17.01.13.png>)

La segunda opción y la más sencilla es que dejes todo el tema de certificados en nuestras manos. Nosotros nos encargaremos de emitirte un certificado válido y renovarlo sin ningún tipo de asistencia o preocupación por tu parte.&#x20;

**Para esto, eso sí, el dominio debe apuntar ya a Transparent Edge Services.**

Los pasos a seguir son sencillos: dentro del [portal](https://dashboard.transparentcdn.com), en la parte de **Autoprovisioning**, haz clic sobre la pestaña de _sites_ y sobre el candado del sitio web que quieras proteger con SSL.

![](<../../.gitbook/assets/Captura de pantalla 2020-05-21 a las 17.09.32.png>)

****

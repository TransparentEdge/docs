# Caching de objetos estáticos vs. dinámicos

Entendemos por objetos estáticos todos aquellos objetos pedidos a un servidor mediante protocolo HTTP o HTTPS que no cambian desde que son creados. Ejemplos de estos objetos estáticos son las imágenes, los textos, los archivos comprimidos (ZIP,TGZ, etc.), hojas de estilo (CSS), JavaScript, XML, etc.

Por contenido dinámico entendemos justo lo contrario: son aquellas llamadas realizadas desde un cliente a un servidor mediante protocolo HTTP o HTTPS que cambian a cada iteración del usuario. Ejemplos de contenido dinámico son una transacción bancaria, una personalización en el HTML de una web o un proceso de compra _online_.

En Transparent Edge somos capaces de cachear tanto contenido estático como contenido dinámico, si bien este último tipo de contenido depende mucho del tipo de web y de la frecuencia de actualización del mismo.

Tomemos, por ejemplo, un _e-commerce_ que tiene su parte privada de usuario en el menú superior derecho, donde aparece un mensaje de bienvenida. Cuando abres sesión en la página, te devuelve una página adaptada a tus preferencias de compra. En esta parte, lo que hacen muchos _e-commerce_ o _sites_ similares con personalización de usuario es directamente no cachear, marcando una cabecera de caché de 0s o enviando _no-cache_ dentro del _Cache-Control_.

Pues bien, en Transparent Edge es posible cachear todo ese contenido si el origen es capaz de identificar mediante una _cookie_ o cualquier otra cabecera si un usuario ha abierto sesión o no. Así, guardaríamos una versión cacheada de la página para usuarios sin abrir sesión (navegación anónima) y una versión de la página por cada uno de los usuarios con sesión abierta.

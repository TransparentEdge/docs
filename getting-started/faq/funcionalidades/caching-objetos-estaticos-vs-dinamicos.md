# Caching de objetos estáticos vs. dinámicos

Por objetos estáticos entendemos todos aquellos objetos pedidos a un servidor mediante protocolo HTTP o HTTPS que no cambian desde que son creados. Ejemplos de estos objetos estáticos son las imágenes, los textos, los archivos comprimidos (zip, tgz, etc.), hojas de estilo (css), javascripts, xml, etc.

Por contenido dinámico entendemos justo todo lo contrario: son aquellas llamadas realizadas desde un cliente a un servidor mediante protocolo HTTP o HTTPS que cambian a cada iteración del usuario. Ejemplos de contenido dinámico son una transacción bancaria, una personalización en el html de una web o un proceso de compra _online_.

En Transparent Edge Services somos capaces de cachear tanto contenido estático como contenido dinámico, si bien este último tipo de contenido depende mucho del tipo de web y de la frecuencia de actualización del mismo.

Tomemos, por ejemplo, un _e-commerce_ que tiene su parte privada de usuario en el lugar en el que en el menú superior derecho aparece un mensaje de bienvenida. Cuando abres sesión en la página, te devuelve una página adaptada a tus preferencias de compra. En esta parte, lo que hacen muchos _e-commerce_ o _sites_ similares con personalización de usuario es directamente no cachear, marcando una cabecera de caché de 0s o enviando no-cache dentro del Cache-Control.

Pues bien, en Transparent Edge Services es posible cachear todo ese contenido si el origen es capaz de identificar mediante una _cookie_ o cualquier otra cabecera si un usuario ha abierto sesión o no, de forma que guardaríamos una versión cacheada de la página para usuarios sin abrir sesión (navegación anónima) y una versión de la página por cada uno de los usuarios con sesión abierta.


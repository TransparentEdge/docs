# Prefetching

El _prefetching_ es una funcionalidad que nos permite precalentar contenido antes de la llegada de los usuarios. Es muy útil en ocasiones. Aquí te ponemos dos ejemplos:

* Tienes un _site_ con poco tráfico pero, aún así, quieres que cuando llegue un usuario y solicite un objeto, este sea servido desde la caché de la CDN.
* Vas a sacar tu nuevo sitio web a producción y esperas una gran afluencia de público que se traduce en un elevado número de peticiones a la CDN. Si la CDN está fría y no tiene los objetos en la caché, cuando salgas "al aire" es posible que tengas un aluvión de peticiones en tus servidores de origen y puedan llegar a colapsarte los sistemas.

Para saber cómo hacer un _prefetch,_ puedes consultar este [enlace](../../dashboard/prefetch.md).

# Refetching

El _refetching_ es una funcionalidad mediante la cual, tras un invalidado de contenido de tipo PURGE, obligas al agente que se ejecuta en los nodos de CDN y se encarga de la invalidación de contenido a pedir el objeto de nuevo. De esta manera, cuando llegue una petición real por parte del usuario, ese objeto ya se encuentrará en la caché y no tendrá que recogerlo del origen.

Puedes realizar un _refetch_ vía [API](https://api.transparentcdn.com/docs) o desde nuestro [_dashboard_](https://dashboard.transparentcdn.com)_,_ como se explica en este [enlace](../../dashboard/invalidando-contenido.md).

{% hint style="warning" %}
El método de invalidación BAN no es compatible con _refetching._
{% endhint %}

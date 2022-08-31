# Actuando sobre el Query String

En Transparent Edge Services implementamos [Varnish Enterprise](https://www.varnish-software.com/) y por lo tanto puedes aprovechar todas las ventajas que esto supone.

En este caso vamos a hablar de como usar el módulo [urlplus](https://docs.varnish-software.com/varnish-cache-plus/vmods/urlplus/)  en Transparent Edge Services para interactuar con el query string de una petición web.

### Normalizando el QS

Es posible que quieras normalizar la URL, en particular sus parametros, por ejemplo el orden que llegan puede ser importante para aumentar la [efectividad de la caché](../getting-started/faq/hitratio.md) y quieras asegurarte que los parametros de la url siempre llegan en el mismo orden independientemente del sitio desde el que se llame

```javascript
sub vcl_recv
{
    urlplus.write();
}
```

En cualquier caso, si no quieres ordenarla basta con que hagas lo siguiente:

```javascript
sub vcl_recv
{
    urlplus.write(sort_query = false);
}
```

{% hint style="info" %}
La método write debes llamarlo siempre que trabajes con el módulo urlplus y quieras alterar la url.
{% endhint %}

### Eliminamos parametros del QS

En determinadas ocasiones nos puede interesar el algún todos los parametros que nos llegue a Transparent Edge Services para mejorar la efectividad de cache o simplemente por que queremos que a origen baje la url sin un parametro en particular. Una forma de hacerlo sería así, en este caso estamos eliminando un parametro con nombre random:

```javascript
sub vlc_recv {
    urlplus.query_delete("random");
    urlplus.write();
}
```



### Eliminando un parametro del QS mediante regex

Al igual que el ejemplo anterior te podría interesar eliminar un parametro o parametros del QS en base a una expresión regular. Por ejemplo podemos quitar todos los parametros que nos introduce Google Analytics:

```javascript
sub vcl_recv
{
    urlplus.query_delete_regex("utm_");
    urlplus.write();
}
```

Otras funciones que pueden serte de utilidad son&#x20;

* **query\_keep.** Para borrar todo el QS salvo un parametro en particular.
* **query\_get**. Para obtener el valor de un parametro del QS.
* **tolower**. Para convertir toda la URL a minúsculas.
* **toupper**. Para convertir toda la URL a mayúsculas.
* **query\_add**. Para añadir un parametro a la url

En este [enlace](https://docs.varnish-software.com/varnish-cache-plus/vmods/urlplus/) puedes encontrar toda la documentación de este módulo de [Varnish Enterprise.](https://www.varnish-software.com/)

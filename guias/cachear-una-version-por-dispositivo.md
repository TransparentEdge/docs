# Cachear una versión por dispositivo

Todas las peticiones que entren a tu sitio web a través de Transparent Edge Services van a ser clasificadas por defecto en función del tipo dispositivo que el cliente ha usado para navegar por tu sitio web.&#x20;

Esta clasificación dejará una cabecera con nombre X-Device que puede contener tres valores distintos:

* **mobile**
* **tablet**
* **desktop**

Ahora bien, para guardar una copia distinta del mismo objeto en función del dispositivo, es necesario que lo configures adecuadamente bien en tus servidores web de origen o bien en Transparent Edge Services.

Para ello, entra en nuestro [portal](https://dashboard.transparentcdn.com), ve al menú **Provisioning, VLC Config** y pincha en la parte avanzada. Duplica la última configuración vigente añadiendo estas lineas dentro de la funcion vcl\_backend\_response y guarda los cambios:

```javascript
sub vcl_backend_response {
    set beresp.http.Vary = "X-Device"
}
```

Está configuración se aplicará a todos los sitios web que tengas dado de alta en tu cuenta de Transparent Edge Services, si solo quieres habilitarlo en algún dominio en particular puedes hacer algo así:

```javascript
if (bereq.http.host == 'www.transparentcdn.com') {
    set beresp.http.Vary = "X-Device";
}
```

Ahora solo afectará solo a ese dominio específico.

{% hint style="warning" %}
si ya están mandando una cabecera Vary desde origen, la configuración arriba indicada sobreescribirá la cabecera Vary con el valor que le indiques, por lo tanto en este caso es necesario que añadas el X-Device a la cabecera Vary ya existente de este modo.
{% endhint %}

```javascript
sub vcl_backend_response {
    set beresp.http.Vary = beresp.http.Vary+",X-Device"
}
```

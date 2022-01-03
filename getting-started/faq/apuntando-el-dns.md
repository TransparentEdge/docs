# Apuntando el DNS

Tras completar el alta del servicio en Transparent Edge Services, te llegará un correo de bienvenida. En él podrás encontrar un [CNAME](glosario/cname-1.md) para terminar la configuración.&#x20;

Dicho CNAME tiene la siguiente estructura:

`<SERVICIO>.<ID_CLIENTE>.edge2befaster.net`

Vamos a ver cómo configurar todo con el siguiente CNAME de ejemplo:

**caching.c4.edge2befaster.net**

Este es el CNAME al que apuntar tus dominios. También lo encontraras visible en todo momento en nuestro [_dashboard_](https://dashboard.transparentcdn.com) en la pestaña de **Provisioning.**

![](<../../.gitbook/assets/Captura de pantalla 2020-06-24 a las 9.16.44.png>)

Una vez tengas claro cuál es tu CNAME asignado, lo único que tienes que hacer es ir a tu proveedor DNS (normalmente es el mismo en el que has comprado tus dominios) y cambiar el registro que quieres pasar por Transparent Transparent Edge Services.&#x20;

Antes de seguir, déjanos apuntar que esta forma de trabajar, mediante CNAMEs, solo es válida para **subdominios**, como _www.tudominio.com_ o _blog.tudominio.com_, nunca para apuntar directamente a _tudominio.com_ sin subdominio. Esta limitación viene impuesta por la [RFC 1912](https://www.ietf.org/rfc/rfc1912.txt) y, aunque tiene excepciones, hay muchos servidores DNS que directamente no te permiten hacerlo.

Volviendo al tema, lo más normal sería que quieras pasar el subdominio _www.tudominio.com._ En este caso, una vez dentro ya de tu proveedor DNS, tienes que localizar el registro **www,** que lo más probable es que apunte a una dirección IP mediante un registro de tipo[ A](https://www.ietf.org/rfc/rfc1912.txt). Algo así:

```
www.tudominio.com	    A	    1.1.1.1
```

Lo que tienes que hacer es eliminar ese registro y crear uno que, en lugar de ser de tipo A, sea de tipo CNAME y apunte a **caching.c4.edge2befaster.net**&#x20;

```
www.tudominio.com    CNAME    caching.c4.edge2befaster.net 
```

Este cambio no es automático y depende de dos cosas principalmente:

1. Del tiempo que tarde tu proveedor DNS en aplicar los cambios. Suele ser casi inmediato.
2. Del tiempo de propagación DNS. Según la RFC, puede tardar hasta 24 horas en verse el cambio, pero por lo general, suelen ser solo unos minutos. Este _delay_ viene dado por la propia arquitectura de los DNS en internet y los tiempos de cacheo asociados a cada dominio.

{% hint style="warning" %}
Es recomendable que cuando vayas a hacer un cambio de DNS en el que quieres que los cambios se vean reflejados en el menor tiempo posible bajes el TTL de tu dominio al menor tiempo posible.
{% endhint %}

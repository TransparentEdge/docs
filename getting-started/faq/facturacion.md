# Modelo de facturación

Nuestro modelo de facturación dependerá del servicio que hayas contratado con Transparent Edge Services, pero hay varios aspectos a entender y que trataremos de explicarte en este apartado de la mejor manera posible.&#x20;

Lo primero y más importante que queremos transmitirte de Transparent Edge Services es que somos una compañía flexible en todos los departamentos, desde el técnico hasta el comercial y, especialmente, en el financiero. Es importante para nosotros que tu experiencia con Transparent Edge Services sea única y que siempre sientas la libertad de transmitirnos tus impresiones. Para cualquier consulta o duda que tengas con tu facturación, puedes ponerte en contacto con tu comercial o puedes enviar un mail al departamento de [Ventas](mailto:sales@transparentedge.eu). También puedes hablar directamente con nuestro equipo de [_billing_](mailto:billing@transparentedge.eu) si lo deseas.

### CDN

{% hint style="success" %}
Artículos facturables:

* GB transferido.
* Player (en caso de necesitarlo).
* Storage (si trabajas en modo _pull_).
{% endhint %}

En estos dos productos facturamos por GB transferidos y todas las [funcionalidades](funcionalidades/) están incluidas en el precio del servicio. Intentamos ser lo más previsibles que podemos con la emisión de tus facturas para evitarte sorpresas, por eso solemos trabajar en un modelo de compromiso de tráfico o _**commitment**_ y un exceso. En base a tu experiencia y las previsiones de tráfico de tu sitio web, puedes contratar un _commitment_ de tráfico con los TB que creas necesarios y será por lo que se te facturará mensualmente. En caso de que un mes excedas ese _commitment_ de tráfico, se te aplicará un cargo sobre el tráfico excedido. Cuanto más grande sea el _commitment_, más barato te saldrá el precio del GB transferido.

{% hint style="info" %}
Pongamos, por ejemplo, que contratas un paquete de 10TB a un precio de 500€ (0,05€/GB) con un exceso de 0,06€/GB.

El primer mes consumes 8TB, por lo que pagarás 500€ del _commitment_.

El segundo mes consumes 11TB. Este mes has tenido un exceso de 1TB. En este caso se te facturarán los 500€ de tu _commitment_ más 1TB de exceso a 0,06€/GB. Por lo tanto, se te facturarán 61,44€ adicionales.&#x20;

Recuerda que 1TB son 1024GB no 1000 **😉.**
{% endhint %}

Como decimos, somos muy flexibles. Si este modelo no te encaja, podemos ir a un modelo de pago por uso puro o incluso podemos regular los excesos de forma trimestral, semestral o anual, en lugar de mensualmente.

{% hint style="warning" %}
Hay determinados artículos de terceros que integramos dentro de nuestro ecosistema, como el _player_ de JW Player, que tienen un precio independiente que no está incluido en el precio del GB.
{% endhint %}

De forma adicional y para simplificar más el proceso hemos creado tres paquetes de servicio. En dos de ellos, además de distribucin de contenido, incluimos un paquete de tráfico WAF y otro paquete de tráfico i3:

![](<../../.gitbook/assets/Captura de pantalla 2022-11-16 a las 9.06.15.png>)

### Edge Transcoding

{% hint style="success" %}
Artículos facturables:

* Minuto de transcodificación (VoD).
* Hora de transcodificación (Live).
* Punto de ingesta (Live).
{% endhint %}

En este producto se factura por minutos de transcodificación cuando hablamos de [VoD](broken-reference) y, por lo general, también se trabaja con un paquete de minutos o en pago por uso. Actualmente no es un servicio que puedas contratar de manera autónoma desde nuestra web. Para hacerlo, puedes ponerte en contacto con nuestro equipo de [Ventas](mailto:sales@transparentedge.eu).

Cuando lo que estamos transcodificando son vídeos o audios en un formato _live_, necesitarás un punto de ingesta donde tus cámaras ingesten la señal para que sea posteriormente distribuida por Transparent Edge o por cualquier otra CDN (podemos trabajar con otra CDN por delante, no hay problema). Cuando la señal está en nuestro punto de ingesta, tenemos la capacidad de transcodificar esa señal para adaptarla en tiempo real a todos los dispositivos. Esta transcodificación será tarificada por horas de transcodificación, no por minutos. Por lo tanto, en esta modalidad de _transcoding_ tienes dos elementos facturables: las horas de transcodificación y el punto de ingesta.

### Edge Security

{% hint style="success" %}
Artículos facturables:

* Millón de peticiones (WAF).
* Servicio AntiDoS (opcional).
{% endhint %}

[Edge Security](facturacion.md#transparent-secure-layer) es nuestro _stack_ de productos de seguridad y está compuesto de varios subproductos. El más importante es nuestro WAF (_web application firewall_), cuyo modelo de facturación se basa en _request_. Se factura por millón de peticiones y regularmente en base a un _commitment_ más un exceso si lo hubiera.

Otra cosa a tener en cuenta cuando contratas este servicio de WAF avanzado es que es una capa extra de la CDN y por eso tu tráfico pasará por la CDN antes de pasar por el WAF, con los costes asociados que eso supone. Normalmente hacemos un _commitment_ conjunto de CDN + WAF para tratar de adaptarnos a tus necesidades.

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com) puedes hacerlo desde nuestro [API](glosario/api.md).
{% endhint %}

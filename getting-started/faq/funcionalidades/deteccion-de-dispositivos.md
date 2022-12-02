# Detección de dispositivos

En la actualidad, Transparent Edge es capaz de distinguir en base al _User-Agent_ el tipo de dispositivo que se está conectando y categorizarlo en escritorio, móvil o tableta.\
Para identificar el dispositivo, Transparent Edge Services incluye una cabecera llamada X-Device que llega en la _request_ a origen . Esta cabecera puede tener tres valores:

* desktop
* mobile
* tablet

En base a está cabecera, Transparent Edge Services puede guardar una versión de cada objeto por dispositivo, lo que hace posible servir diferente contenido en función del dispositivo bajo la misma URL. Si quieres saber cómo hacerlo, [aquí](../../../guias/cachear-una-version-por-dispositivo.md) te lo explicamos.

### Forzado de dispositivo&#x20;

Aún habiendo detectado el dispositivo, es posible forzar que un dispositivo móvil, por ejemplo, muestre una versión de escritorio. Esto lo hacemos mediante la _cookie_ _Force-Device_. Si esta _cookie_ está presente, tiene prioridad sobre _X-Device_.


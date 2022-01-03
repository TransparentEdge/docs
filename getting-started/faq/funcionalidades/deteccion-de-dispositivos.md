# Detección de dispositivos

En la actualidad, Transparent Edge Services es capaz de distinguir en base al User-Agent el tipo de dispositivo que se está conectando y categorizarlo en escritorio, móvil o _tablet_.\
Para identificar el dispositivo, Transparent Edge Services incluye una cabecera llamada X-Device que llega en la _request_ a origen . Esta cabecera puede tener tres valores:

* desktop
* mobile
* tablet

En base a está cabecera, Transparent Edge Services puede guardar una versión de cada objeto por dispositivo, siendo posible servir diferente contenido en función del dispositivo bajo la misma URL. Si quieres saber cómo hacerlo, [aquí](../../../guias/cachear-una-version-por-dispositivo.md) te lo explicamos.

### Forzado de dispositivo&#x20;

Aún habiendo detectado el dispositivo, es posible forzar que un dispositivo móvil, por ejemplo, muestre una versión de escritorio. Esto lo hacemos mediante la _cookie_ "Force-Device". Si esta _cookie_ está presente, tiene prioridad sobre el X-Device.

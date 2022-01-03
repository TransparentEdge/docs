# X-Device

Transparent Edge Services es capaz de distinguir en base al User-Agent el tipo de dispositivo que se está conectando y categorizarlo en escritorio, móvil o _tablet_.\
Para identificar el dispositivo, Transparent Edge Services incluye una cabecera llamada X-Device, que llega tanto en la _request_ a origen como en la _response_ al cliente. Esta cabecera puede tener tres valores:

* desktop
* mobile
* tablet

En base a esta cabecera, Transparent Edge Services guarda una versión de cada objeto por dispositivo, lo que hace posible servir contenido diferente en función del dispositivo bajo la misma URL.[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Detecci%C3%B3n\_de\_dispositivos\_m%C3%B3viles/edit?section=2)

### Forzado de dispositivo (funcionalidad bajo petición)

Aun habiendo detectado el dispositivo, es posible forzar que un dispositivo móvil, por ejemplo, muestre una versión de escritorio. Esto lo hacemos mediante la _cookie_ "Force-Device". Si esta _cookie_ está presente, tiene prioridad sobre el X-Device.

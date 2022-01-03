# Restricciones de seguridad

En Transparent Edge Services existe una serie de restricciones de seguridad cuando intentas aprovisionar tu configuración VCL a través de nuestro portal de autoprovisioning y escribir tu propio código VCL.&#x20;

Estas restricciones tienen la finalidad de garantizar la seguridad y la estabilidad de todos los _sites_ que pasan por Transparent Edge Services.&#x20;

### Funciones predefinidas&#x20;

Varnish, en cualquiera de sus versiones, te permite sobreescribir el comportamiento de sus funciones predefinidas. Sin embargo, en Transparent Edge Services solo permitimos la reescritura de las siguientes [funciones](funciones-por-defecto.md):

* vcl\_recv
* vcl\_hash
* vcl\_miss
* vcl\_deliver
* vcl\_backend\_fetch
* vcl\_backend\_response

### Return

La función **return** en Varnish es utilizada típicamente para saltar las diferentes subrutinas predefinidas o, incluso, las definidas por el usuario. Sin embargo, en Transparent Edge Services no podemos pemitir su uso porque por una mala intención o el desconocimiento podrían derivar en problemas funcionales y de estabilidad de la plataforma.

### Definición de funciones personalizadas

Aunque estamos trabajando para cambiar este punto, en la actualidad no se permite crear funciones personalizadas por parte del usuario desde el portal de _provisioning_. Sin embargo, sí puedes subir esas funciones escritas por ti con la ayuda de nuestro equipo [técnico](mailto:soporte@transparentedge.eu).

### Call

Enlazando con el punto anterior, como ocurre con el **return,** tampoco permitimos el uso de la función **call,** que en teoría permite llamar a funciones previamente deficnidas en el VCL.



Para nosotros es importante que tengas la mayor autonomía posible y seas capaz de hacer todas las cosas que necesites en nuestro entorno. Por eso, nuestra plataforma está en constante evolución. Si echas en falta algo, estaremos encantados de ayudarte a implementarlo.



{% hint style="warning" %}
Que tú no lo puedas hacer algo desde el portal no quiere decir que no se pueda implementar. Si ves que no puedes hacer algo que necesitas por alguna de nuestras restricciones, no dudes en ponerte en contacto con nuestro equipo de [soporte](mailto:support@transparentcdn.com), que te ayudará a implementar tu configuración en Transparent CDN&#x20;
{% endhint %}

# Fast purging

De forma predeterminada, en Transparent Edge Services implementamos un sistema de purgado de contenido instanteo o _fast purging_. Este sistema nos permite invalidar contenido en todos los nodos de Transparent Edge Services en menos de dos segundos.

#### ¿Cómo implementamos _fast purgin_?

Es muy sencillo. Todos los nodos de Transparent Edge Services están suscritos a una cola de tipo _fanout_ en un clúster interno de Rabbit MQ. Cuando a este llega un mensaje de invalidado de contenido procedente de nuestra [API](../glosario/api.md), el mensaje es consumido de forma inmediata por un agente que se ejecuta en todos los nodos de la CDN y se encarga de invalidar localmente el contenido solicitado en ese nodo.

Si quieres saber más sobre cómo invalidar contenido, puedes ir a este [enlace](../../dashboard/invalidando-contenido.md).

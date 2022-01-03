# Plugin para wordpress

**W3 Total Cache** es un plugin de wordpress que permite el uso de Transparent Edge Services como pull cdn.&#x20;

**Instalacion y configuracion:**&#x20;

Para descargar el plugin debemos dirigirnos a la sección del panel de administración de wordpress destinada a tal efecto. La encontraremos en el desplegable lateral bajo la leyenda de "plugins". Una vez ahí pulsaremos en "Add plugins". En el buscador situado en la parte superior izquierda introduciremos w3 total cache.

![](<../.gitbook/assets/001 (2).png>)

Una vez que tengamos el plugin instalado tendremos que activarlo. Simplemente haremos clic en activar y tendremos w3-total-cache activado en nuestro sitio web.

![](<../.gitbook/assets/002 (1).png>)

Ya con el plugin activado procederemos a su configuración. Veremos como tendremos disponible bajo la pestaña "Performance" del menú lateral el submenú "General Settings". Nos dirigiremos a este apartado para configurar el plugin con los parámetros de Transparent Edge Services.

![](<../.gitbook/assets/003 (2).png>)

En esta pantalla, en la sección "FSD CDN", poner check en "Enabled" y seleccionar Transparent CDN. No es relevante el campo que elijamos en el desplegable "CDN".

![](../.gitbook/assets/012.png)

En la misma pantalla, bajo la configuracion de "Page cache", nos aseguramos de que también esté marcado el "Enable" de "Page cache", guardamos con "Save all settings"**.**

![](../.gitbook/assets/013.png)

A continuación nos vamos al submenú "CDN" dentro de "Performance". En esta pantalla, configuraremos los parametros de la cuenta de transparent que tengamos asignados. Si no sabe cuales son sus parámetros de acceso, pongase en contacto con el servicio técnico. Una vez que configuremos nuestros parámetros podremos probar que son correctos. Nos aparecerá un mensaje indicándonoslo o, por el contrario, otro mensaje diferente si debemos corregirlos.

![](../.gitbook/assets/014.png)

![](../.gitbook/assets/015.png)

Por último, para terminar la configuración, en la seccion "Page cache" bajo "Purge policy", nos aseguramos de marcar todas las secciones que queramos descachear automáticamente cada vez que se publique un nuevo post o se actualice uno existente.

![](../.gitbook/assets/016.png)

Una vez seguidos estos pasos, el plugin ya está configurado y funcional. Puede operar con el blog de la manera habitual y el plugin se encargará de notificar los cambios a Transparent CDN para que actualice las copias guardadas.


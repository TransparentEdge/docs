# Backends

Aquí podrás ver todos los _backends_ configurados, modificarlos, añadirlos o eliminarlos.

Si has seguido el asistente inicial, ya tendrás al menos un _backend_ configurado y verás algo similar al siguiente ejemplo:

![](<../../../.gitbook/assets/image (36).png>)

Para cada _backend_ se muestran unos detalles básicos, así como el estado de su _health check._ En el campo de acciones, podrás eliminarlo, modificarlo o visualizar su configuración.

### Alta de nuevo _backend_

Dar de alta un nuevo _backend_ en la plataforma no lleva más de cinco minutos. Pulsa en "Añadir backend" y se mostrará un diálogo como el siguiente:

![](<../../../.gitbook/assets/image (37).png>)

Tendrás entonces que rellenar los siguientes apartados básicos:

* **Nombre**: Elige un nombre sencillo que ayude a identificar al _backend._ Va siempre prefijado por un código único, asignado automáticamente en el alta de autoprovisionamiento. Será el nombre usado en las configuraciones VCL.
* **IP Origen**: La dirección IP o el dominio que apunta al servidor de origen. En el caso de usar un dominio, la única restricción es que no puede coincidir con el dominio definido en [_Sitios_](https://docs.transparentcdn.com/getting-started/dashboard/autoprovisionamiento/sitios-o-dominios)_._
* **Puerto**: __ El puerto a usar para conectar a origen. Suele ser 80 o 443.
* **SSL**: Marca esta opción si la comunicación entre Transparent Edge Services y el servidor de origen debe ir cifrada.

Ahora, en el apartado "Configurar Health Check", especificaremos la petición que realizaremos desde Transparent Edge Services para monitorizar el estado del _backend_:

* **Host**: Especifica el dominio y subdominio que ayude a identificar al servidor de origen el sitio web solicitado donde se aloja el _health check_ (ejemplo: _miweb.ejemplo.es_).
* **URL de comprobación**: La URL a la que apuntaremos no es normalmente la portada principal del sitio, sino un pequeño fichero, html o ruta que contenga un _index.html_  liviano (ejemplo: _/check_).
* **Código de respuesta**: El que debería devolver la llamada al _health check_ normalmente es 200, pero podría ser cualquier código que garantice que el _backend_ está funcionando ([más información sobre códigos de respueta HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Status)).

Con esto ya tendríamos el _backend_ configurado. Restaría pulsar_Guardar_ para aplicar los cambios y poder usarlo ya en las configuraciones VCL.

### Eliminar, modificar o visualizar un _backend_.

Para cada _backend_ configurado, en el campo de Acciones tenemos disponibles los siguientes botones para eliminar, modificar y visualizar la configuración:

![](<../../../.gitbook/assets/image (38).png>)

Antes de eliminar o modificar un _backend_ (en caso de cambiarle el nombre), asegúrate de que ninguna configuración VCL dependa de él o dejará de funcionar correctamente.

{% hint style="danger" %}
Una cosa que tienes que tener en cuenta es que si vas a configurar tu backend por ssl, tienes que un backend ssl por cada site. Si quieres compartir un backend que habla con el origen por ssl hay que desactivar la opción de SNI y eso es algo que solo se puede hacer bien via [API ](../../faq/glosario/api.md)o contactando con nosotros a través de nuestro equipo de [soporte](mailto:support@transparentcdn.com)
{% endhint %}

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [dashboard](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../../faq/glosario/api.md).
{% endhint %}

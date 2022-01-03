# Sitios o dominios

En este apartado podrás ver todos los sitios configurados, añadir nuevos y activar la gestión automática de certificados SSL si lo deseas.

Si has seguido el asistente inicial, ya tendrás al menos un sitio configurado y verás algo similar al siguiente ejemplo:

![](<../../../.gitbook/assets/image (39).png>)

Aquí se mostrarán todos los sitios configurados. Si tienes muchos, puedes filtrar con el buscador ubicado a la izquierda y ver cuáles tienen activada la gestión automática de certificados (en ese caso, el candado estará cerrado).

{% hint style="warning" %}
Un detalle importante es el aviso de la parte superior, que nos indica dónde tendremos que [apuntar el CNAME](https://docs.transparentcdn.com/getting-started/faq/apuntando-el-dns) del sitio. Aunque para que todo funcione correctamente, primero terminaremos toda la configuración relacionada en la plataforma de autoprovisión.
{% endhint %}

### Añadir Sitio

Añadir un sitio nuevo es un proceso sencillo, pero para validar que realmente controlas el sitio web, se deberá dejar un fichero de texto en la raíz del servidor web. Este proceso se explica detalladamente cuando pulsas sobre _**Añadir sitio**_,. Si tienes cualquier problema, no dudes en contactar con nosotros en [soporte@transparentedge.eu](mailto:soporte@transparentedge.eu)

![](<../../../.gitbook/assets/image (40).png>)

Una vez creado dicho fichero, pulsa _Guardar_ y tu sitio quedará registrado en la plataforma.

### Gestión automática de certificados

Si lo prefieres, podemos llevar la gestión de certificados automáticamente desde nuestra plataforma. Los certificados de tu sitio estarán firmados en ese caso por [Let's Encrypt](https://letsencrypt.org).

Para activar esta opción, pincha en _Acciones_ sobre el candado del sitio que quieras activar (para que funcione el CNAME, nos debe estar apuntando).

![](<../../../.gitbook/assets/image (41).png>)

![](<../../../.gitbook/assets/image (43).png>)

Ahora solo tendrás que pulsar sobre _**Quiero gestionar automáticamente el SSL para este site**_ para iniciar el proceso. No tardará más de cinco minutos en activarse.

![El candado se mostrará de color verde para un sitio activo.](<../../../.gitbook/assets/image (44).png>)

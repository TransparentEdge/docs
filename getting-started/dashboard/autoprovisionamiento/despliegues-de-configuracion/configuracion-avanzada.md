# Modo avanzado

Nuestro _dashboard_ ofrece la posibilidad de añadir configuración VCL personalizada, otorgando un mayor control respecto al modo básico, junto con un historial de configuraciones.

Para ello, cambia el modo de "Básico" a "Avanzado":

![](<../../../../.gitbook/assets/image (19).png>)

En la imagen anterior, puedes ver el histórico de configuraciones. Si aún no tienes ninguna, mostrará "No hay datos".

![](<../../../../.gitbook/assets/image (51).png>)

Selecciona "Añadir configuración VCL" y aparecerá una ventana emergente donde podrás editar el código VCL o bien pegar uno que ya tengas guardado en tu equipo.

![](<../../../../.gitbook/assets/image (65).png>)

Tras editar la configuración, pulsa en "Guardar". El código entrará en un flujo de validaciones y, si todo es correcto, se aplicará en producción.

{% hint style="info" %}
El despliegue de la configuración no es inmediato, pero como máximo puede tardar 10 minutos contando desde que se guarda o aplica una configuración en el _dashboard_.
{% endhint %}

Puedes consultar el estado en el historial de configuraciones. Aparecerá una nueva entrada con la fecha de alta y te mostrará si ya está desplegado o no en producción.

![](<../../../../.gitbook/assets/image (18).png>)

Cuando aparezca el _tic_ verde en validado y estado, sabremos que la nueva configuración está aplicada.

Siguiendo con las opciones del historial de configuraciones, a la derecha, bajo "Acciones", tenemos los siguientes botones:

![](<../../../../.gitbook/assets/image (7).png>)

* **Ver:** Nos proporciona una vista de solo lectura del código.
* **Duplicar:** Duplica la entrada seleccionada y nos permite editarla y aplicarla si deseamos.
* **Volver a producción:** Se usa para aplicar una entrada antigua en producción. Es muy útil para dar marcha atrás a un cambio.

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro __ [_dashboard_](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../../../faq/glosario/api.md).
{% endhint %}

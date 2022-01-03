# Modo básico

El modo básico nos permite configurar de manera rápida las reglas VCL más comunes, una redirección permanente, ver las reglas existentes, editarlas...

La primera vez que accedemos al panel no tendremos ninguna regla definida:

![](<../../../../.gitbook/assets/image (47).png>)

### Añadir una regla

Añadir una nueva regla es tan fácil como hacer clic sobre la plantilla deseada en el menú de la derecha. Por ejemplo, vamos a establecer una regla de _Redirección permanente (301)_.

Tendremos que rellenar los siguientes campos, que pueden variar según la plantilla elegida:

**Condición**:

* **Nombre**: Un identificador para esta regla.
* **Sitio web**: Seleccionaremos mediante un desplegable a qué sitio web de los que tenemos configurados aplicar esta regla. También podemos aplicarla a todos.
* **Campo**: Seleccionaremos, mediante un menú desplegable, el campo al que afectará esta regla.
* **Operador**: Seleccionaremos nuevamente un operador que se aplicará entre el _Campo_ y el _Valor_.
* **Valor**: Escribe aquí el valor a comparar contra el _Campo_ y el _Operador._ Por ejemplo, si el campo es _request URL_, aquí indroducirás dicha URL.

**Acción**:

* **Plantilla**: No hace falta modificar nada, viene pre-seleccionada cuando pulsamos sobre la plantilla en cuestión.
* **Campos extra:** Este campo será distinto según la plantilla elegida. En el caso de redirección permanente (301), veremos el campo **attr** y tendremos que especificar a dónde se hará la redirección.

**Ejemplo**:

![](<../../../../.gitbook/assets/image (48).png>)

Una vez tengamos todo listo, pulsamos sobre _Guardar_.

### Ver y desplegar reglas

Cuanto tengamos definidas varias reglas, podemos ver todas ellas desde el panel, donde podremos borrarlas o modificarlas:

![](<../../../../.gitbook/assets/image (49).png>)

Una vez tengamos todo tal y como queremos, podemos pulsar sobre _**Validar y desplegar reglas.**_  Estas entrarán en un flujo de validación y despliegue.

{% hint style="info" %}
El despliegue de la configuración no es inmediato, pero como máximo puede tardar 10 minutos contando desde que se guarda o aplica una configuración en el _dashboard._
{% endhint %}

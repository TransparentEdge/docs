# Configuración inicial

Tras haber concluido el proceso de registro y activado nuestra cuenta de usuario, podrás acceder al panel (_dashboard_) de autoprovisionamiento de Transparent Edge Services a través de la URL [https://dashboard.transparentcdn.com/](https://dashboard.transparentcdn.com) con tu usuario (tu correo de registro) y tu contraseña definida en el proceso de registro.&#x20;

Así, en tu primer inicio de sesión, serás redireccionado a un asistente (_wizard_) que te guiará, paso por paso, durante todo el proceso de configuración de la plataforma. Este asistente podrá ser invocado en un futuro siempre que desees. Por ejemplo, cuando sea preciso añadir un nuevo dominio (_site_), eliminar aquellos que ya no sean necesarios o, en general, realizar cualquier otro tipo de modificación en la configuración existente.

Además de dicho asistente, tendrás a tu disposición dos modos de configuración manual, uno básico y otro avanzado.

### Asistente de configuración (_wizard_)

El asistente de configuración consta de cuatro pasos que deberás completar a fin de hacer efectivos nuestros ajustes.

#### Configurar el _backend_

En primer lugar, será necesario definir el _backend._ Se trata del servidor (_host_) externo a la CDN donde actualmente se aloja el sitio web que vamos a configurar en la plataforma de autoprovisionamiento.

A nivel de CDN, el _backend_ es su origen.

Los datos requeridos para la correcta configuración del _backend_ son los siguientes:

![](<../../../.gitbook/assets/image (24).png>)

#### Nombre

Deberás proporcionar un nombre descriptivo para el _backend_.

Este será empleado en las distintas configuraciones VCL (Varnish Configuration Language) que indiquemos posteriormente. Está precedido por el prefijo c\[company\_id]\_, siendo \[company\_id] tu identificador único de usuario en la plataforma de autoprovisiamiento de Transparent Edge Services.

Por ejemplo, un nombre de _backend_ podría ser: cNNN\_ejemplo.

#### IP de origen

Se trata de la IP pública del servidor externo a la CDN donde actualmente se aloja el sitio web.

En caso de disponer de un nombre de dominio para dicho servidor, este también puede ser empleado en vez de su IP. La única restricción al respecto es que el nombre de dominio para el origen no sea el mismo que el empleado más adelante en la configuración del sitio web.

Por ejemplo, un nombre de dominio de origen podría ser: origen.ejemplo.com.

#### Cifrado SSL

Activaremos este _checkbox_ para indicar que la comunicación con el _backend_ ha de ser cifrada.

#### Puerto

Complementa a la IP o el nombre de dominio de origen previamente establecidos.

Se trata del puerto a través del cual se establecerá la conexión con dicho origen.

Por lo general, en caso de que la conexión sea no cifrada, se utilizará el protocolo HTTP (Hypertext Transfer Protocol), cuyo puerto estándar es el 80. No obstante, si se requiere una conexión cifrada, será necesario el protocolo HTTPS (HTTP Secure), cuyo puerto estándar es el 443.

#### _Health check_

Debe ser configurado un _health check_ para monitorizar el estado del _backend_. Se precisan, a tal efecto, las siguientes variables:

#### _Host_

Cabecera (_header_) de _host_ asociada al _health check_.

Se trata de un campo opcional, por lo que puede permanecer vacío o, por el contrario, puede contener cualquier valor admitido por parte del servidor de origen.

Por ejemplo, una cabecera de host asociada válida podría ser: health\_check.ejemplo.com.

#### URL de comprobación

Se trata de la URL que comprobará el health check.

Por ejemplo, una URL de comprobación válida podría ser: /check.

#### Código de respuesta

Código de respuesta (_status code_) esperado para la comprobación del _health check_.

Por ejemplo, un código de respuesta para la comprobación del health check podría ser 200; esto es lo más habitual (status code; OK) aunque no existe restricción alguna a este respecto, siendo posible, por ejemplo, un código de respuesta 301 (status code: MOVED PERMANENTLY).

#### Alta del Dominio

Tras cumplimentar aquellos datos relativos a la configuración del _back-end_, tendremos que dar de alta el sitio web que se asociará a dicho _backend_. Lo haremos en el siguiente paso haciendo clic en "Añadir sitio".

Por ejemplo, un sitio web podría ser: [www.ejemplo.com](http://www.ejemplo.com).

![](<../../../.gitbook/assets/image (25).png>)

A fin de asegurar que el sitio web indicado nos pertenece, se nos ordenará que situemos un archivo con el nombre tcdn.txt en la raíz de dicho sitio web y con un contenido específico (15e3fa052eec7e562214b54459f8c890), de tal modo que una petición del tipo [http://www.ejemplo.com/tcdn.txt](http://www.ejemplo.com/tcdn.txt) devuelva el texto previamente facilitado. En caso de cualquier problema y/o duda a este respecto, puedes ponerte en contacto con Transparent CDN a través de la dirección de correo electrónico [soporte@transparentcdn.com](mailto:soporte@transparentcdn.com).

#### VCLs

Una vez ha sido cumplimentada en el asistente de configuración (_wizard_) __ la información referente tanto al origen (_backend_) como al dominio (_site_), se nos mostrará un extracto con la configuración VCL (Varnish Configuration Language) generada.

VCL (Varnish Configuration Language) no es más que un lenguaje de _script_ utilizado para configurar y agregar lógica a la caché de Varnish.&#x20;

Por ejemplo, los valores de _backend_ y _site_ referidos previamente desembocarían en la siguiente configuración VCL:

sub vcl\_recv {

&#x20;   if (req.http.get == «www.ejemplo.com») {&#x20;

&#x20;       set req.backend\_hint = cNNN\_ejemplo.backend();&#x20;

&#x20;   }&#x20;

}

Como puede observarse, esta no es más que una configuración inicial en la que se vincula el _backend_ cNNN\_ejemplo, cuyo origen, recordemos, es origen.ejemplo.com con el _site_ [www.ejemplo.com](http://www.ejemplo.com). Posteriormente se llevarán a cabo sucesivas modificaciones en dicha configuración a través de los modos disponibles, que son básico y avanzado. Los describimos a continuación.

![](<../../../.gitbook/assets/image (26).png>)

#### Resumen

Por último, el asistente de configuración te informará de las modificaciones que deberás llevar a cabo en los ajustes de DNS para apuntar el dominio previamente indicado en el registro CNAME (Canonical NAME) de Transparent Edge Services.

Por ejemplo, nuestro registro CNAME asignado sería: caching.cNNN.edge2befaster.net.

Este CNAME lo podrás ubicar tanto en el correo de activación como en la parte superior del panel de Autoprovisionamiento.



{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro __ [_dashboard_](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../../faq/glosario/api.md).
{% endhint %}


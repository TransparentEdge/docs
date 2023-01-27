# Certificados SSL

En este apartado podrás ver qué certificados has añadido a la plataforma, así como modificarlos o solicitar uno nuevo.

Podrás gestionar tus certificados de tres formas:

* [Personalizados](ssl.md#certificados-personalizados)
* [Autogenerados HTTP](ssl.md#certificados-autogenerados-http)
* [Autogenerados DNS](ssl.md#certificados-autogenerados-dns)

Los certificados personalizados son aquellos que ya posees o has obtenido por otros medios y serás el encargado de mantenerlos actualizados.

Los certificados autogenerados se generan mediante procesos internos de la CDN y se renuevan automáticamente.

### Certificados personalizados

Si ya dispones de un certificado digital para tu dominio, puedes incorporarlo a la CDN fácilmente, tan sólo debes tener a mano dicho certificado en formato [PEM](https://es.wikipedia.org/wiki/X.509#Extensiones\_de\_archivo\_de\_certificados).

Para poder importar un certificado, tendrás que tener listos dos ficheros: uno con la cadena completa del certificado y otro con la clave privada, ambos codificados en Base64 (normalmente llevan la extensión _.pem_ o _.crt_). Muchos proveedores de certificados suelen dar varias opciones en cuanto al formato a la hora de descargar el certificado.

Podrás saber si el certificado tiene el formato correcto si al abrirlo con un editor de texto puedes ver los campos "BEGIN CERTIFICATE" y "END CERTIFICATE".

En el caso de que debas concatenar la cadena completa del certificado por tu cuenta, la forma correcta de hacerlo es la siguiente:

```
-----BEGIN CERTIFICATE-----
(El certificado primario: tu_dominio.crt)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
(El certificado intermedio: CA.crt)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
(El certificado raíz: TrustedRoot.crt)
-----END CERTIFICATE-----
```

En el caso de la clave privada, la estructura es la siguiente:

```
-----BEGIN PRIVATE KEY-----
(Clave privada: private.key)
-----END PRIVATE KEY-----
```

Una vez lo tengas listo, accede a nuestro panel:

**Aprovisionamiento > Certificados > Nuevo > Certificado personalizado**

Aparecerá una ventana donde podrás pegar el contenido de la clave pública y privada de tu certificado.

Si el certificado es correcto una vez pulses en "Crear" podrás ver el certificado añadido en la tabla del panel y se desencadenará un proceso interno en la CDN que desplegará dicho certificado en toda la plataforma, este proceso no tarda más de 5 minutos, tras lo cual los dominios que encajen con dicho certificado lo usarán automáticamente.

El proceso de renovación es igual de sencillo, podrás ver en el panel qué día caducará el certificado para planearlo de antemano (también recibirás una notificación por correo), básicamente consiste en editar el certificado actual (haciendo clic en el icono del lápiz).

{% hint style="warning" %}
Sólamente es posible editar los certificados personalizados, el resto de certificados (autogenerados HTTP y autogenerados DNS), tan sólo se pueden eliminar o leer, ya que se gestionan y renuevan internamente.
{% endhint %}

### Certificados autogenerados HTTP

Este tipo de certificado es autogestionado por la CDN, tanto su expedición inicial como las renovaciones son gestionadas automáticamente por la CDN siempre que se cumpla un requisito fundamental: **el dominio debe apuntar a nivel DNS a la CDN**.

Para obtener un certificado se debe crear una "Certificate Request":

**Aprovisionamiento > Certificados > Nuevo > Solicitud de certificado (desafío HTTP)**

Selecciona los dominios que quieres que formen parte del certificado, opcionalmente puedes marcar la casilla "Standalone", esto implica que este certificado no podrá formar parte de una SAN con otros certificados de tu compañía, recomendamos no seleccionarla a no ser que sea necesario.

Al pulsar "Crear" se creará la "Certificate Request" y se mostrará el histórico de peticiones,  en dicho histórico podrás consultar el estado de tu solicitud.

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Puedes consultar el histórico de peticiones en cualquier momento:

**Aprovisionamiento > Certificados > Opciones > Histórico de peticiones HTTP**

### Certificados autogenerados DNS

Este tipo de certificado al igual que el HTTP es gestionado por la CDN, la diferencia está en que los certificados se generan mediante un desafío DNS.

Esto implica que no es estrictamente necesario que los dominios apunten a la CDN y tiene una ventaja muy grande: se pueden solicitar certificados [wildcard](https://en.wikipedia.org/wiki/Wildcard\_certificate).

Requisitos:

* Un proveedor DNS compatible (puedes consultar la lista de proveedores en la gestión de credenciales, que veremos más adelante)
* Poseer las claves necesarias para actualizar registros DNS

Si has delegado el DNS a Transparent Edge, no tienes que disponer de ninguna clave y los requisitos anteriores se cumplen automáticamente, sólo tendrías que crear un credencial con el proveedor "[Transparent Edge DNS](ssl.md#si-el-proveedor-dns-es-transparent-edge)".

#### 1.1 Credenciales - (ejemplo con AWS Route53)

Antes de nada tenemos que crear las credenciales necesarias para actualizar registros DNS en nuestro proveedor, en este ejemplo AWS Route53, o cómo mínimo, el registro [ACME](https://en.wikipedia.org/wiki/Automatic\_Certificate\_Management\_Environment) necesario, que en nuestro ejemplo sería `_acme-challenge.example.com`.

**Aprovisionamiento > Certificados > Opciones > Gestión de credenciales DNS**

Se mostrará una tabla con las credenciales existentes, haz clic en "Nueva Credencial"

Introduce un alias para identificar a este credencial y selecciona el proveedor DNS, en este ejemplo "AWS (Route53)"

Aparecerán automáticamente los campos necesarios para la credencial, esto difiere entre proveedores - consulta la documentación del proveedor en caso de duda o contacta con nosotros.

Una vez hayas finalizado haz clic en "Crear".

Puedes editar o eliminar credenciales desde este mismo apartado, recuerda que las renovaciones de los certificados generados mediante desafío DNS utilizarán las credenciales asignadas con los valores que tengan en ese momento.

<figure><img src="../../../.gitbook/assets/image (22) (2).png" alt=""><figcaption></figcaption></figure>

#### 1.2 Nueva solicitud de certificado DNS - (ejemplo con AWS Route53)

Ahora que ya contamos con los credenciales procederemos a crear la solicitud de certificado:

**Aprovisionamiento > Certificados > Nuevo > Solicitud de certificado (desafío DNS)**

Se mostrará un asistente donde deberás escribir los dominios que quieres que contemple el nuevo certificado, en este ejemplo queremos "`example.com`" y "`*.example.com`", escribimos cada uno en una línea, así:

```
example.com
*.example.com
```

En el siguiente apartado debemos seleccionar los credenciales que irán asociados a esta "Solicitud de certificado DNS", hay que tener en cuenta que estas solicitudes son permanentes y se reutilizan cuando llega la hora de renovar el certificado, por lo que si cambian los credenciales éstos se deben actualizar como corresponda.

Una vez creada la solicitud de certificado, se mostrará una tabla con las peticiones DNS activas y podrás consultar el estado de la solicitud.

Si finalmente se genera el certificado aparecerá automáticamente en la tabla del apartado de certificados.

<figure><img src="../../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

#### Si el proveedor DNS es Transparent Edge

En el caso de que hayas delegado la gestión de los DNS a nuestra CDN no necesitarás tener a mano ningún credencial aunque sí que tendrás que crearlo, debes seguir los pasos habituales para crear una nueva credencial y seleccionar "Transparent Edge DNS".

El campo que aparecerá lo puedes dejar vacío o escribir cualquier texto, no tendrá uso alguno.

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../../faq/glosario/api.md).
{% endhint %}

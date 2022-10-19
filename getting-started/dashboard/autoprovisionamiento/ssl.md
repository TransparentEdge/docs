# Certificados SSL/TLS

En este apartado podrás ver qué certificados propios has añadido a la plataforma, así como modificarlos o solicitar uno nuevo.

Podrás gestionar tres tipos de certificados:

* Personalizados
* Autogenerados HTTP
* Autogenerados DNS

Los certificados personalizados son aquellos que ya posees o has obtenido por otros medios y serás el encargado de mantenerlos actualizados.

Los certificados autogenerados se generan mediante procesos internos de la CDN y se renuevan automáticamente.

### Certificados personalizados

Si ya dispones de un certificado digital para tu dominio, puedes incorporarlo a la CDN fácilmente, tan sólo debes tener a mano dicho certificado en formato [PEM](https://es.wikipedia.org/wiki/X.509#Extensiones\_de\_archivo\_de\_certificados).

Para poder importar un certificado, tendrás que tener listos dos ficheros: uno con la cadena completa del certificado y otro con la clave privada, ambos codificados en Base64 (normalmente llevan la extensión _.pem_ o _.crt_). Si tu proveedor es Digicert, te suele dar varias opciones a la hora de descargar el certificado.

Podrás saber si el certificado tiene el formato correcto si al abrirlo con un editor de textos puedes ver los campos "BEGIN CERTIFICATE" y "END CERTIFICATE".

En el caso de que debas concatenar la cadena completa del certificado por tu cuenta, la forma correcta de hacerlo es la siguiente:

```
-----BEGIN CERTIFICATE-----
(El certificado primario: tu_dominio.crt)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
(El certificado intermedio: DigiCertCA.crt)
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
(El certificado raíz: TrustedRoot.crt)
-----END CERTIFICATE-----
```

En el caso de la clave privada, la estructura es la siguiente:

```
-----BEGIN PRIVATE KEY-----
Clave privada: private.key
-----END PRIVATE KEY-----
```

A continuación se detalla el proceso a seguir para agregar un nuevo certificado personalizado o renovarlo:

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Si el certificado es correcto una vez pulses en "Crear" podrás ver el certificado añadido en la tabla del panel y se desencadenará un proceso interno en la CDN que desplegará dicho certificado en toda la plataforma, este proceso no tarda más de 5 minutos, tras lo cual los dominios que encajen con dicho certificado lo usarán automáticamente.

El proceso de renovación es igual de sencillo, podrás ver en el panel qué día caducará el certificado para planearlo de antemano (también recibirás una notificación por correo), básicamente consiste en editar el certificado actual (haciendo clic en el icono del lápiz), como se muestra a continuación:

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Sólamente es posible editar los certificados personalizados, el resto de certificados (autogenerados HTTP y autogenerados DNS), tan sólo se pueden eliminar o leer, ya que se gestionan y renuevan internamente.
{% endhint %}

### Certificados autogenerados HTTP

Este tipo de certificado es autogestionado por la CDN, tanto su expedición inicial como las renovaciones son gestionadas automáticamente por la CDN siempre que se cumpla un requisito fundamental: **el dominio debe apuntar a nivel DNS a la CDN**.

Para obtener un certificado se debe crear una "Certificate Request":

1. Accede al apartado de "Certificados"
2. Haz clic en "Nuevo ..."
3. Haz clic en "Solicitud de certificado (desafío HTTP)"

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

4\. Selecciona los dominios que quieres que formen parte del certificado

5\. Opcionalmente, marca la casilla "Standalone", esto implica que este certificado no podrá formar una SAN con otros certificados de tu compañía, recomendamos no seleccionarla a no ser que sea necesario.

6\. Pulsa en "Crear", esto creará la "Certificate Request" y se mostrará el histórico de peticiones, dicho histórico te mostrará el estado de tu solicitud.

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Puedes consultar el histórico de peticiones en cualquier momento:

1. "Opciones ..."
2. "Histórico de peticiones HTTP"

### Certificados autogenerados DNS

Este tipo de certificado, al igual que el HTTP, es autogestionado por la CDN pero los certificados se generan mediante un desafío DNS.

Esto implica que no es estrictamente necesario que los dominios apunten a la CDN y tiene una ventaja muy grande: se pueden solicitar certificados [wildcard](https://en.wikipedia.org/wiki/Wildcard\_certificate).

Requisitos:

* Proveedor DNS compatible (puedes consultar la lista de proveedores en la gestión de credenciales, que veremos más adelante)
* Poseer las claves necesarias para actualizar registros DNS

Si has delegado el DNS a Transparent Edge Services, no tienes que disponer de claves y los requisitos anteriores se cumplen automáticamente.

#### 1.1 Credenciales - (ejemplo con AWS Route53)

Antes de nada tenemos que crear las credenciales necesarias para actualizar registros DNS en nuestro proveedor, en este ejemplo AWS Route53, o cómo mínimo, el registro [ACME](https://en.wikipedia.org/wiki/Automatic\_Certificate\_Management\_Environment) necesario, que en nuestro ejemplo sería `_acme-challenge.example.com`.

1. Accede al apartado "Certificados"
2. Haz clic en "Opciones ..."
3. Haz clic en "Gestión de credenciales DNS"

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

4\. Se mostrará una tabla con las credenciales existentes, haz clic en "Nueva Credencial"

5\. Introduce un alias para identificar a este credencial y selecciona el proveedor DNS, en este ejemplo "AWS (Route53)"

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

6\. Se mostrarán automáticamente los campos necesarios para la credencial, esto difere entre proveedores - consulta la documentación del proveedor en caso de duda o contacta con nosotros.

7\. Una vez hayas rellenado los campos requeridos haz clic en "Crear"

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Puedes editar o eliminar credenciales desde este mismo apartado, recuerda que las renovaciones de los certificados generados mediante desafío DNS utilizarán las credenciales asignadas con los valores que tengan en ese momento.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

#### 1.2 Nueva solicitud de certificado DNS - (ejemplo con AWS Route53)

Ahora que ya contamos con los credenciales procederemos a crear la solicitud de certificado.

1. Haz clic en "Nuevo ..."
2. Selecciona "Solicitud de certificado (desafío DNS)

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Se mostrará un asistente donde deberás escribir los dominios que quieres que contemple el nuevo certificado, en este ejemplo queremos "`example.com`" y "`*.example.com`":

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

En el siguiente apartado debemos seleccionar los credenciales que irán asociados a esta "Solicitud de certificado DNS", hay que tener en cuenta que estas solicitudes son permanentes y se reutilizan cuando llega la hora de renovar el certificado, por lo que si cambian los credenciales éstos se deben actualizar como corresponda.

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>



Una vez creada la solicitud de certificado, se mostrará una tabla con las peticiones DNS activas y podrás consultar el estado de la solicitud.

Si finalmente se genera el certificado aparecerá automáticamente en la tabla del apartado de certificados.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### Si el proveedor DNS es Transparent Edge Services

En el caso de que hayas delegado la gestión de los DNS a nuestra CDN no necesitarás tener a mano ningún credencial aunque sí que tendrás que crearlo, debes seguir los pasos habituales para crear una nueva credencial y seleccionar "Transparent Edge Services DNS".

El campo que aparecerá lo puedes dejar vacío o escribir cualquier texto, no tendrá uso alguno.

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../../faq/glosario/api.md).
{% endhint %}

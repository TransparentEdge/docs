# Certificados comerciales

En este apartado podrás ver qué certificados propios has añadido a la plataforma, así como modificarlos o añadir uno nuevo.

En caso de que prefieras gestionar por tu cuenta los certificados de uno, varios o todos tus sitios, aquí podrás hacerlo cómodamente.

### Requisitos

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

### Añadir certificado

Ahora que tenemos listos ambos ficheros, pulsamos sobre _**Añadir certificado personalizado**_ y pegamos la cadena completa pública en el campo de texto izquierdo y la privada en el derecho, tal y como se indica:

![](<../../../.gitbook/assets/image (45).png>)

Al pulsar en _**Guardar**_, ya tendremos todo listo y el nuevo certificado quedará activo.

{% hint style="info" %}
Los certificados que añadas aquí tendrán prioridad sobre los automáticos y deberás renovarlos manualmente cuando vayan a caducar.
{% endhint %}

### Renovar un certificado

El proceso de renovación es idéntico al de añadir un certificado, salvo que ya tienes el registro del certificado creado, y tendrás que pulsar bajo _Acciones_, en el icono del lápiz, para modificarlo.

![](<../../../.gitbook/assets/image (46).png>)

Puedes consultar la fecha de caducidad del certificado en este mismo panel, en el campo Expiración.

Y recuerda que, por supuesto, siempre puedes optar por la opción de que [gestionemos los certificados automáticamente mediante Let's Encrypt](https://docs.transparentcdn.com/getting-started/dashboard/autoprovisionamiento/sitios-o-dominios).

{% hint style="info" %}
Recuerda que todo lo que puedes hacer desde nuestro [_dashboard_](https://dashboard.transparetncdn.com), puedes hacerlo desde nuestro [API](../../faq/glosario/api.md).
{% endhint %}

---
description: HTTPS
---

# HTTPS

## Gestión automática de certificados

Si no tienes un certificado SSL para tu site, Transparent Edge Services puede gestionar uno automáticamente por ti. Solo tienes que habilitarlo en el panel de autoprovision, en la sección de Sites:

![Sacando certificados automáticamente](<../.gitbook/assets/image (17).png>)

Al pulsar el candado, se abrirá un cuadro de diálogo con instrucciones y los requisitos para la gestión automática:

![Requisitos para la gestión automática de certificados](<../.gitbook/assets/image (55).png>)

Una vez sacado el certificado, el candado se volverá verde y ya puedes despreocuparte. En unos minutos estará desplegado y se renovará automáticamente sin ninguna intervención por tu parte.



## Certificados custom

Al margen de los certificados autogestionados por Transparent Edge Services, puedes subir tus propios certificados a la plataforma, por ejemplo wildcards. Para ello, basta con acceder a la parte de "Certificados" dentro del dashboard de autoprovision, y pulsar sobre el botón de "Añadir certificado personalizado". En el cuadro emergente, se introducirá en la parte izquierda el certificado en formato PEM (son texto y comienzan con la cadena ------BEGIN CERTIFICATE), con las CA, si las hubiera, concatenadas. En la parte derecha, se introducirá la clave privada del certificado (también texto, suelen comenzar por ---------BEGIN PRIVATE KEY), y al guardar (previa validación del mismo) se almacenará el certificado y se desplegará en unos minutos

## Protocolos

En Transparent Edge Services ofrecemos seguridad en las conexiones adecuadas al estado del arte, mediante el protocolo TLS. Soportamos tanto TLSv1.2 como TLSv1.3, habiendo quedado obsoletas todas las versiones anteriores siguiendo las recomendaciones de la RFC 8996.

## Cifrados

La _suite_ de cifrados usada en Transparent Edge Services es estándar y está en permanente adaptación a las buenas prácticas aceptadas comúnmente en ciberseguridad, proporcionando un equilibrio adaptado entre la compatibilidad con el mayor número de dispositivos y la seguridad del cifrado, incluyendo los siguientes:

```
"EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH"
```

## Características soportadas

Las siguientes _features_ son soportadas por los terminadores de Transparent Edge Services:

* _**TLS Resumption**_: Usamos tickets e identificadores de sesión para implementarlo. Esto hace el TTFB mucho menor para visitantes recurrentes.
* _**OCSP Stapling**_: Para acelerar lo más posible la validación de los certificados por parte del cliente.
* _**HSTS**_: El uso de Varnish permite que la cabecera se pueda añadir por VCL en la subrutina _vcl\_deliver_ aunque el origen no la esté incluyendo.
* _**Perfect Forward Secrecy**_: Incorporamos Diffie-Helman para el uso de PFS.

# HTTPS

## Automatic Certificate Management

If you don't have an SSL certificate for your site, Transparent Edge Services can manage one for you automatically. You just need to enable it in the self-provisioning panel, either in the Sites section or in the Certificates section.

In the case of doing it from the Sites section, you simply need to click on the lock icon that appears on the right side of the list. This will open a dialog box with instructions and requirements for the automatic management:

<figure><img src="../.gitbook/assets/certificado-http.png" alt=""><figcaption><p>New certificate</p></figcaption></figure>

In a few minutes, it will be deployed and automatically renewed without any intervention on your part.

## Custom Certificates

In addition to the certificates managed by Transparent Edge Services, you can upload your own certificates to the platform, such as wildcard certificates. To do this, simply access the "Certificates" section within the autoprovision dashboard and click on the "Add Custom Certificate" button. In the popup box, you will enter the certificate in PEM format on the left side (it is a text and starts with the string "-----BEGIN CERTIFICATE"), with any intermediate CAs concatenated if applicable. On the right side, you will enter the private key of the certificate (also text, usually starting with "-----BEGIN PRIVATE KEY"), and upon saving (after validation), the certificate will be stored and deployed within a few minutes.

## Protocolos

At Transparent Edge Services, we provide state-of-the-art security for connections through the TLS protocol. We support both TLSv1.2 and TLSv1.3, following the recommendations of RFC 8996, which rendered all previous versions obsolete.

## Ciphers

The cipher suite used in Transparent Edge Services is standard and constantly evolving to align with commonly accepted cybersecurity best practices. It offers a balanced approach between compatibility with a wide range of devices and encryption security. Some of the supported ciphers include:

```
"EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH"
```

## Supported Features&#x20;

The following features are supported by Transparent Edge terminators:

* **TLS Resumption:** We utilize session tickets and session identifiers to implement TLS resumption. This significantly reduces the time to the first byte (TTFB) for returning visitors.
* **OCSP Stapling:** We implement OCSP stapling to expedite certificate validation by the client.
* **HSTS:** With the use of Varnish, the HSTS header can be added via VCL in the _vcl\_deliver_ subroutine, even if the origin server does not include it.
* **Perfect Forward Secrecy:** We incorporate Diffie-Hellman for the implementation of PFS.

\

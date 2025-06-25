# Initial configuration

After activating our user account, you will be able to access the Transparent Edge panel through the URL [https://dashboard.transparentcdn.com/](https://dashboard.transparentcdn.com/auth/login?redirect=%2F) using your username (registered email) and password defined during the registration process.

### Configuring the Backend

The backend must first be defined. +This is the external server (host) where the website that we are configuring on the auto-provisioning platform is currently hosted. At CDN level, the backend is your origin.

The following data are required for the correct configuration of the backend:

<figure><img src="../../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

#### Name:

You must provide a descriptive name for the backend.&#x20;

This name will be used in the different VCL (Varnish Configuration Language) configurations indicated later. It is preceded by the prefix `c[company_id]_`, where `[company_id]` is your unique user identifier in the Transparent Edge Services auto-provisioning platform.&#x20;

For example, a backend name could be: `cNNN_example`.

#### Source IP

This is the public IP of the external server where the website is currently hosted.&#x20;

If you have a domain name for that server, it can also be used instead of its IP. The only restriction is that the domain name for the origin cannot be the same as the one used later in the website configuration.&#x20;

For example, an origin domain name could be: source.example.com.

#### SSL Encryption

This checkbox is ticked to indicate that communications with the backend must be encrypted.

#### Port

This complements the previously established origin IP or domain name.&#x20;

It is the port through which the connection to the origin will be established.&#x20;

Generally, if the connection is unencrypted, the HTTP (Hypertext Transfer Protocol) will be used, the standard port of which is 80. However, if an encrypted connection is required, HTTPS (HTTP Secure) will be required, the standard port of which is 443.

#### Health check

A health check must be configured to monitor the backend status. The following variables are required for this purpose:

#### Host

Header of the host associated with the health check.

This is an optional field, so it can be left blank or it can contain any value accepted by the origin server.

For example, a valid associated host header could be: health\_check.example.com.

#### Verification URL

This is the URL that the health check will verify.

For example, a valid verification URL could be: /verify.

#### Status code

Expected status code for the health check verification.

For example, a status code for the health check verification could be 200; this is the most common (status code: OK), although there is no restriction on this. For instance, a response code 301 (status code: MOVED PERMANENTLY) is also possible.

#### Domain Registration

After completing the data related with the backend configuration, the website to be associated with that backend must be registered. This is done in the next step by clicking on "Add site".

Let's see this with an example domain: `www.example.com`.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

To ensure the indicated website belongs to us, we will be asked to place a file named `tcdn.txt` in the root directory of the website with a specific content (the verification code), so that a request of the type `http://www.example.es/tcdn.txt` returns the text previously provided.&#x20;

Alternatively, we can create a TXT record with the name "`_tcdn_challenge`" and the verification code content.

<figure><img src="../../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

If you encounter any problems or doubts in this regard, you can contact Transparent Edge via email at [soporte@transparentedge.eu](mailto:soporte@transparentcdn.com).

#### VCLs

Once the information regarding both the backend and the domain (site) has been completed in the configuration wizard, an excerpt with the generated VCL (Varnish Configuration Language) configuration will be displayed.

VCL (Varnish Configuration Language) is simply a script language used to configure and add logic to the Varnish cache.

For example, the backend and site values previously referred to would result in the following VCL configuration:

```

sub vcl_recv {
    if (req.http.host == "www.example.es") {
        set req.backend_hint = cNNN_ejemplo.backend();
    }
}
```

As can be seen, this is just an initial configuration that links the backend `cNNN_example` with the site `www.example.es`. This allows the CDN to retrieve non-cached resources (MISS).

You can end the wizard here and complete the VCL configuration later.

#### Summary

Finally, the configuration wizard will inform you of the modifications you will need to make in the DNS settings to point to the domain previously indicated in the CNAME (Canonical NAME) record of Transparent Edge.

For example, our assigned CNAME record would be: `caching.cNNN.edge2befaster.net.`

You can find this CNAME record in both the activation email and at the top of the Auto-provisioning panel.

{% hint style="info" %}
Remember that every single task you can do from our [dashboard](https://dashboard.transparentcdn.com/) can also be accomplished from our [API](../../faq/glosario/api.md).
{% endhint %}

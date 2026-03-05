# Sites

In this section, you'll be able to view/add or delete the sites managed by our CDN.

Provisioning -> Sites

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2026-03-05 a las 11.45.39.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
An important notice that you can see in this section is the CNAME where you must point your sites to your DNS configuration.
{% endhint %}

### Adding a new site

Adding a new site requires a verification of ownership of the domain being added, which is a security measure. If you have any doubts or need assistance, don't hesitate to contact our support [help+cdn@transparentedge.eu](mailto:help+cdn@transparentedge.eu).

The verification process can be carried out in two ways:

**Verification via HTTP request**

You must create a `tcdn.txt` file at the root of your site with the verification code provided by the system. Once done, through the URL we provide you can check that the verification code appears in it. DNS can only be modified once this verification has been completed.

<figure><img src="../../../.gitbook/assets/site-1.png" alt=""><figcaption></figcaption></figure>

**DNS record verification**

In this case the system provides a unique code or text that must be copied. With it, you need to add a new TXT record in the place where the domain management is carried out. This way our system will query your domain's DNS and confirm that you are the owner by finding said record.

### Delegate the rest of the configuration?

From this point on, if you wish, we can take care of everything else.

<figure><img src="../../../.gitbook/assets/Captura de pantalla 2026-03-05 a las 10.21.28.png" alt=""><figcaption></figcaption></figure>

\
If you choose to do so, we will handle the **backend configuration** through VCL settings — that is, specifying the origin server where the website is hosted. We will also take care of setting up the **HTTP to HTTPS redirect**.

{% hint style="warning" %}
In the event that your site has not been verified within 24 hours after having selected this option, all the configuration created automatically will be deleted.
{% endhint %}

**What if my domain is on another CDN?** You will simply need to confirm the CDN migration and we will take care of everything else.

If you choose to do it yourself, remember that it is important to carry out the same steps manually.

### What is the lock icon next to my sites?

This is our legacy method to obtain certificates, please refer to [this section of the documentation](ssl.md).

### I can't verify my site / I have to add a lot of sites at once

If you have any problems adding new sites, please contact our support. We can add sites in bulk and/or bypass the verification if there are any issues: [help+cdn@transparentedge.eu](mailto:help+cdn@transparentedge.eu).

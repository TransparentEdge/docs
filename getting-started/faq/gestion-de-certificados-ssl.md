# SSL

When required, Transparent Edge can do SSL terminations by caching requests as if they were in HTTP. In this case, the traffic is sent encrypted between the user and Transparent Edge. The certificate is then decrypted in our caches and all the traffic within the Transparent Edge platform is sent in HTTP. This lets us apply web caching technology to the SSL traffic. The traffic between Transparent Edge and the client's origin server can be sent in HTTP or HTTPS, at the client’s discretion.

By default, we use HTTP2 on tlsv1.2 and tlsv1.3.

## Automatic management of SSL

At Transparent Edge, there are always several different ways of doing something. In this case, we give you two options for serving a secure website.

The first is for you to purchase your certificates from your trusted provider, and once you have them you can upload them to our platform or send us a [ticket](<mailto:support@transparentedge.eu >) so we can do it for you.

To upload certificates to the platform, go to our[ client portal](https://dashboard.transparentcdn.com/), select Provisioning from the left side menu, and then click on **Certificates**.

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

The second, and simpler, option is to leave the entire matter of certificates to us. We’ll take care of sending you valid certificates and renewing them, so you don’t have to do or worry about anything.

#### For this to happen, the domain does have to be pointing to Transparent Edge.

It’s easy: in the Provisioning section of the[ portal](https://dashboard.transparentcdn.com/), click on the Sites tab and then on the padlock icon for the website you want to protect with SSL.

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>


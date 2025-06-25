# DNS Pointing

After you sign up for Transparent Edge, you’ll receive a welcome email. It will include a[ CNAME](https://docs.transparentedge.eu/v/english/getting-started/faq/glosario/cname-1) to complete the configuration.

The CNAME will have the following structure:

`<SERVICIO>.<ID_CLIENTE>.edge2befaster.net`

We’re going to look at how to do the configuration using this CNAME as an example:

**caching.c4.edge2befaster.net**

This is the CNAME to point your domains at. You can also always find it in the Provisioning tab of our[ dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F).

![](<../../.gitbook/assets/Captura de pantalla 2020-06-24 a las 9.16.44.png>)

Once you know your assigned CNAME, you just have to go to your DNS provider (normally it’s where you bought your domains) and change the record you want to pass through Transparent Edge.

Before we continue, we’d just like to mention that this way of working, using a CNAME, is only valid for **subdomains** like www.yourdomain.com or blog.yourdomain.com, never to point directly at yourdomain.com without a subdomain. This limitation is imposed by the[ RFC 1912](https://www.ietf.org/rfc/rfc1912.txt) and, although there are exceptions, many DNS servers simply don’t allow it.

Typically, you’ll want it to be the subdomain www.yourdomain.com. In this case, once you’re in your DNS provider, you’ll have to find the **www** record, which most likely points to an IP address with an[ A](https://www.ietf.org/rfc/rfc1912.txt) record. Something like this:

```
www.tudominio.com	    A	    1.1.1.1
```

You’ll have to delete that record and create a new one that is a CNAME instead of an A record, pointing to **caching.c4.edge2befaster.net.** Such as:

```
www.tudominio.com    CNAME    caching.c4.edge2befaster.net 
```

This change does not happen automatically and how long it takes primarily depends on two things:

1. The time your DNS provider takes to apply the changes, which is usually immediate.
2. The DNS propagation time. According to the RFC, it can take up to 24 hours to see the change, but it generally only takes a few minutes. This delay is due to the DNS architecture itself and the caching times of each domain.

{% hint style="warning" %}
If you want to make a DNS change where the changes appear as quickly as possible, we recommend lowering your domain’s TTL as much as you can.
{% endhint %}

# Blocking by IP Address

{% hint style="info" %}
To block longer lists of IP addresses, please check [network ACLs](../getting-started/dashboard/auto-provisioning/network-acls.md).
{% endhint %}

To block traffic coming from one or multiple IP addresses, you can incorporate the following configuration snippet within the vcl\_recv function.

```javascript
sub vcl_recv{
    if (req.http.True-Client-Ip ~ "(1.1.2.2|1.2.3.4)") { 
        call deny_request;
    } 
}

```

In this example, we use the IP address provided in the [True-Client-Ip](../getting-started/faq/cabeceras-por-defecto/true-client-ip-y-x-forwarded-for.md) header. If the request comes from any of the listed IP addresses, it will be blocked immediately.

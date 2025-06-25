# Caching a version per device

All requests entering your website through Transparent CDN will be classified by default based on the device type the client used to browse your website.&#x20;

This classification will leave a header named X-Device, which can have three different values:

* **mobile**
* **tablet**
* **desktop**

In order to cache a separate copy of the same object based on the device, it is necessary to configure it properly either on your origin web servers or on Transparent CDN.

To do this, log in to our [dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F), go to the Provisioning menu, **VLC Config**, and click on the advanced section. Duplicate the last active configuration and add these lines within the vcl\_backend\_response function, then save the changes:

```javascript
sub vcl_backend_response {
    set beresp.http.Vary = "X-Device"
}
```

This configuration will be applied to all websites that you have registered in your Transparent CDN account. If you only want to enable it for a specific domain, you can do something like this:

```javascript
if (bereq.http.host == 'www.transparentcdn.com') {
    set beresp.http.Vary = "X-Device";
}
```

&#x20;Now it will only affect that specific domain.

{% hint style="warning" %}
If you are already sending a Vary header from the origin, the configuration mentioned above will overwrite the Vary header with the value you specify. Therefore, in this case, you need to add the X-Device to the existing Vary header as follows.
{% endhint %}

```javascript
sub vcl_backend_response {
    set beresp.http.Vary = beresp.http.Vary+",X-Device"
}
```

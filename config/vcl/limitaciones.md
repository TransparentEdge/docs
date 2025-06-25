# Security restrictions

There are several security restrictions in Transparent CDN when you try to provision your VCL configuration through our self-provisioning portal and write your custom VCL code.&#x20;

These restrictions aim to ensure the security and stability of all sites that pass through Transparent CDN.

### Predefined functions&#x20;

In any version of Varnish, you can overwrite the behavior of its predefined functions; however, in Transparent CDN, we only allow the rewriting of the following [functions](funciones-por-defecto.md):

* vcl\_recv
* vcl\_hash
* vcl\_miss
* vcl\_deliver
* vcl\_backend\_fetch
* vcl\_backend\_response

### Return

The **return** function in Varnish is typically used to bypass different predefined or even user-defined subroutines. However, in Transparent CDN, we cannot allow the use of this function, as its malicious or unintentional use could lead to functional and platform stability issues.

### Custom function definition

Although we are working to change this point, currently, users are not allowed to create custom functions from the provisioning portal. However, we can upload those functions written by you with the help of our [team](mailto:support@transparentcdn.com).

### Call

Linking to the previous point, similar to return, we do not allow the use of the **call** function, which theoretically allows calling previously defined functions in VCL. Only a set of functions are allowed and you can view them [here](callable-functions.md).

It is essential for us that you have the greatest possible autonomy and can perform all the tasks you need in our environment. That's why our platform is continuously evolving. If there's something you miss or need, we'll be delighted to help you implement it.



{% hint style="warning" %}
However, just because you can't do it from the portal doesn't mean it can't be implemented. If you find that you can't do something you need due to one of our restrictions, don't hesitate to contact our [support](mailto:support@transparentcdn.com) team. They will assist you in implementing your configuration in Transparent CDN.&#x20;
{% endhint %}

# Network ACLs

{% hint style="info" %}
You can check more about how to configure a Network ACL [here](../../../config/network-access-control-list/)
{% endhint %}

## How to use a Network ACL in Auto Provisioning

First, take note of the name of the Network ACL, for example `acl_c4_mylist`.

Now, create a new VCL configuration cloning the last one.

Modify and adapt one of the below examples for your use case.

### Deny list example

```perl
# Deny list example
sub vcl_recv {
    if (req.http.host == "www.mydomain.com") { # any required condition to trigger the ACL check
        if (aclplus.match(client.ip, network_acl.get("acl_c4_mydenylist", "none"))) {
            # Any action is allowed here, for this example we block the request
            call deny_request;
        }
    }
}
```

Use the following conditional to combine multiple deny lists together:

```perl
if (aclplus.match(client.ip, network_acl.get("acl_c4_deny1", "none"))
        || aclplus.match(client.ip, network_acl.get("acl_c4_deny2", "none"))
   ) {
    # Block the request if the IP is present in any ACL
    call deny_request;
}
```

### Allow list example

Here we just inverted the condition to transform this into an allow list (only the IPs present in the ACL will be accepted)

```perl
# Allow list example (we just inverted the condition, notice the '!')
sub vcl_recv {
    if (req.http.host == "www.mydomain.com") { # any required condition to trigger the ACL check
        if (!aclplus.match(client.ip, network_acl.get("acl_c4_myallowlist", "none"))) {
            # Any action is allowed here, for this example we block the request (if the IP doesn't match the ACL)
            call deny_request;
        }
    }
}
```

Use the following conditional to combine multiple allow lists together:

```perl
if (!aclplus.match(client.ip, network_acl.get("acl_c4_allow1", "none"))
        && !aclplus.match(client.ip, network_acl.get("acl_c4_allow2", "none"))
   ) {
    # Block the request if the IP is not present in any of the ACLs
    call deny_request;
}
```

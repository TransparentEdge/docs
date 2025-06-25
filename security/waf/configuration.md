# Configuration

This advanced implementation of WAF will protect your websites more effectively.&#x20;

To activate the WAF, you just need to enable our `TCDN-WAF-Enabled` header.&#x20;

For example, if you wanted to activate the WAF on your domain `mi-dominio.es`, you would simply deploy a [VCL](../../config/vcl/vcl-objects.md) configuration from the [dashboard](../../getting-started/dashboard/) similar to the following:

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
    }
}
```

## Conditional deactivation

If you want to deactivate the WAF under certain conditions, you can simply `unset` the previously assigned header.

For example, if you want to activate the WAF for your domain `mi-dominio.es` but exclude URLs that start `with /path/sin/waf/`, you can deploy a [VCL](../../config/vcl/vcl-objects.md) [configuration](broken-reference) similar to the following from the control [dashboard:](../../getting-started/dashboard/)

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/sin/waf/") {
            unset req.http.TCDN-WAF-Enabled;
        }
    }
}

```

However, this is far from being the best option.

Instead, the WAF provides the header `TCDN-WAF-Set-SecRuleEngine`, which allows us to adjust the behavior of the rule engine. This header accepts three values:

* `#On:` This is the default behavior where the WAF takes necessary actions to block requests considered dangerous.
* `#Off:` Temporarily deactivates the WAF.
* `#DetectionOnly:` In this case, the WAF takes necessary actions to identify requests considered dangerous, but allows them to pass through the WAF. This behavior is useful for conducting preliminary testing to detect potential false positives and subsequently include any necessary exceptions if needed.

Thus, going back to the previous example, we just need to deploy a [VCL](../../config/vcl/vcl-objects.md) [configuration](broken-reference) similar to the following from the [dashboard](../../getting-started/dashboard/):

```javascript
 # WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/sin/waf/") {
            set req.http.TCDN-WAF-Set-SecRuleEngine = "#Off";
        }
    }
}
```

## Including exceptions

If you notice that the WAF is considering certain requests as dangerous, even though they are perfectly valid, and you encounter false positives, you can include exceptions for such cases using the `TCDN-WAF-Allow-Rule-Exceptions` header.

Continuing with the previous example, if you observe that requests to URLs under `/path/completely/secure/` are being blocked by the WAF due to rule violations (`ruleID_1, ruleID_2, ruleID_3, ..., ruleID_n`), you can specify that these matches should be treated as exceptions. To do so, you can deploy the following [VCL](../../config/vcl/vcl-objects.md) [configuration](broken-reference) from the [dashboard](../../getting-started/dashboard/):

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/sin/waf/") {
            set req.http.TCDN-WAF-Set-SecRuleEngine = "#Off";
        }
        if (req.url ~ "^/path/completamente/seguro/") {
            set req.http.TCDN-WAF-Allow-Rule-Exceptions = "ruleID_1 ruleID_2 ruleID_3 ... ruleID_n";
        }
    }
}
```

These are just small examples of very specific use cases. If you have any questions regarding how to integrate this functionality into your own domain, please don't hesitate to contact us at [help+cdn@transparentedge.eu](mailto:help+cdn@transparentedge.eu).

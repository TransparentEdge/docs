---
description: Subroutines to handle requests
---

# Callable Functions

## Overview

These functions enhance the functionality of handling HTTP requests in various ways, including automatically redirecting to HTTPS, denying requests or bypassing cache.

#### NOTE: Deprecated `TCDN-Command` Header

Previously, tasks were triggered using the HTTP header `TCDN-Command`. For example:

* Redirect HTTP to HTTPS: `set req.http.TCDN-Command = "redirect_https";`
* Deny a request with a 403 error: `set req.http.TCDN-Command = "deny_request";`
* Bypass the cache: `set req.http.TCDN-Command = "pass";`
* Apply rate limit: `set req.http.TCDN-Command = "limit_rate:<key>:<limit>:<period>[:<block>][:captcha]";`

However, this method had drawbacks, such as the risk of the header being overwritten later in the VCL code, leading to bugs.

## Available Functions

### **Deny Request**

* **Command**: `call deny_request;`
* **Description**: Immediately blocks the request with a 403 error.
* **Example**:

```perl
sub vcl_recv {
    if (req.url ~ "^/blocked") {
        call deny_request;
    }
}
```

### **Redirect HTTP to HTTPS**

* **Command**: `call redirect_https;`
* **Description**: Redirects HTTP requests to HTTPS.
* **Example**:

```perl
sub vcl_recv {
    call redirect_https;
}
```

### **Bypass Cache**

* **Command**: `call bypass_cache;`
* **Description**: Bypasses/ignores the cache for the current request.
* **Example**:

{% code fullWidth="false" %}
```perl
sub vcl_recv {
    if (req.url ~ "^/dynamic-content") {
        call bypass_cache;
    }
}
```
{% endcode %}

### **Redirect Request**

* **Command**: `call redirect_request;`
* **Description**: Redirects a request to a specified URL with the given status code. It requires setting a header, `req.http.tcdn-location`, before the call. The value of this header must follow the format `<status_code>, <URL>`.
* **Example**:

```perl
sub vcl_recv {
    if (req.http.host == "example.com" && req.url ~ "^/old-section") {
        # req.http.tcdn-location header is required for this call to work.
        set req.http.tcdn-location = "301, https://www.example.com/new-section/";
        call redirect_request;
    }
}
```

### **Apply Rate Limit**

* **Command**: `call rate_limit;`
* **Description**: Applies the rate limit specified, `<limit>` / `<period>` , for each `<key>`. If exceeded, a 429 (Too Many Requests) status code is returned during the `<block>` time indicated.
* **Example**:

```perl
sub vcl_recv {
    if (req.http.host == "example.com" && req.url ~ "^/path") {
        # 'TCDN-WAF-Set-RateLimit-Key' header is optional.
        # If no one is specified, 'True-Client-IP' applies as default.
        set req.http.TCDN-WAF-Set-RateLimit-Key = req.http.True-Client-IP;
        # 'TCDN-WAF-Set-RateLimit-Options' header is mandatory.
        # Syntax: <limit>:<period>[:<block>][:captcha|:js_challenge]
        set req.http.TCDN-WAF-Set-RateLimit-Options = "10:60s:300s";
        call rate_limit;
    }
}
```

### **Under Attack Mode**

* **Command**: `call under_attack;`
* **Description**: Enables Under Attack Mode conditionally, allowing to target only a particular URL or any other condition instead of the whole domain.
* **Example**:

```perl
sub vcl_recv {
    # Keep Under Attack Mode enabled if the domain is 'example.com'
    # and the URL starts with '/admin'
    if (req.http.host == "example.com" && req.url ~ "^/admin") {
        call under_attack;
    }
}

sub vcl_recv {
    # Under Attack Mode can also be activated only if a particular
    # ratelimit is exceeded.
    # To do that, define two headers before calling 'under_attack':
    # - TCDN-UAM-Activation-RateLimit-Key
    # - TCDN-UAM-Activation-RateLimit-Options
    
    # The syntax for TCDN-UAM-Activation-RateLimit-Options is:
    # <limit>:<period>:<uam_duration>
    # It's mandatory to specify a duration value for <period> and <uam_duration>
    # s -> seconds
    # m -> minutes
    # h -> hours

    # This example activates Under Attack Mode for 30s if there are
    # more than 2000 requests to the same host in 1s.
    set req.http.TCDN-UAM-Activation-RateLimit-Key = req.http.host;
    set req.http.TCDN-UAM-Activation-RateLimit-Options = "2000:1s:30s";
    call under_attack;
    
    # Be very careful, the 'call under_attack' instruction above doesn't have 
    # any 'if' conditional surrounding it. It relies solely on the rate limit headers.
    # If those headers aren't correctly defined, the Under Attack Mode will be 
    # enabled unconditionally.
}
```

### **Show Captcha**

* **Command:** `call show_captcha;`
* **Description:** This command triggers the display of a CAPTCHA to verify that incoming traffic is from human users.
* **Example:**

```perl
sub vcl_recv {
    if (req.http.host == "www.example.com" && req.http.geo_country_code != "ES") {
        # Display a CAPTCHA for requests originating outside of Spain.
        call show_captcha;
    }
}
```

### **Show JSChallenge**

* **Command**: `call show_jschallenge;`
* **Description**: This command initiates an automated JavaScript challenge to verify that incoming traffic originates from consumer browsers and not from automated or other tools. Unlike a CAPTCHA, this challenge is unassisted, meaning the user does not need to perform any actions for the verification to occur.
* **Example**:

```perl
sub vcl_recv {
    if (req.http.host == "www.example.com" && req.http.geo_country_code != "ES") {
        # Display a JavaScript challenge for requests originating outside of Spain.
        call show_jschallenge;
    }
}
```

### **BotM Assessment**

* **Command**: `call botm_assessment;`
* **Availability:** This command is available **only** to customers who have the bot mitigation service enabled. Additionally, the affected domain **must** be activated under the bot mitigation panel for this to work.
* **Description**: This command retrieves advanced information about the IP address accessing your service. Based on this data, you can define a more tailored reaction to the request, such as blocking the IP, showing a CAPTCHA, or allowing the request with custom thresholds.
* **Example**:

```perl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        # Default action
        set req.http.TCDN-BM-Action = "block";

        if (req.url ~ "^/posts") {
            # Call BotM manually to retrieve information about the IP
            # This DISABLES the default action defined at 'TCDN-BM-Action'
            call botm_assessment;

            # We can retrieve the score and other parameters about the IP
            if (var.get_int("botm-risk") > 50) {
                call show_captcha;
            } else if (var.get_int("botm-risk") > 15) {
                call show_jschallenge;
            }

            if (var.get("botm-is-abuse") == "1" && var.get_int("botm-risk") > 20) {
                call deny_request;
            }

            # Available variables:
            # 0-99                   var.get_int("botm-risk")
            # 0/empty=false, 1=true  var.get("botm-is-abuse")
            # 0/empty=false, 1=true  var.get("botm-is-anonymous-proxy")
            # 0/empty=false, 1=true  var.get("botm-is-anonymous-vpn")
            # 0/empty=false, 1=true  var.get("botm-is-forum-account-abuse")
            # 0/empty=false, 1=true  var.get("botm-is-reputation")
            # 0/empty=false, 1=true  var.get("botm-is-tor")
            # 0/empty=false, 1=true  var.get("botm-is-automated-navigation")
            # -1-99 (-1=no-dc)       var.get_int("botm-dc-risk") -> datacenter
            # 0-99                   var.get_int("botm-as-risk") -> ASN (autonomous system number)
        }
    }
}
```

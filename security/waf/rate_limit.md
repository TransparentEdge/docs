# Rate limit

This function allows us to limit the number of requests made to our website. This restriction can be based on the client's IP address `(True-Client-IP)`, a cookie, a URL, or anything else that makes sense for your application.&#x20;

This function is invoked through the call `rate_limit`. It requires two headers to be defined before: `TCDN-WAF-Set-RateLimit-Key` and `TCDN-WAF-Set-RateLimit-Options`. The first one, `TCDN-WAF-Set-RateLimit-Key`, is optional and simply corresponds to the identifier we use to name the rule we are configuring; if nothing is specified, `True-Client-IP` is used instead. `TCDN-WAF-Set-RateLimit-Options`, however, is mandatory and it has the following syntax: _`<limit>`_`:`_`<period>`_`[:`_`<block>`_`][:captcha|:js_challenge]`.  _`<limit>`_ determines the maximum number of requests that will be accepted within the  _`<period>`_ specified . Optionally, the parameters _`<block>`_ and `captcha` or `js_challenge` allows us to set the duration during which requests will be denied once the initial limit is reached and display a [CAPTCHA](captcha.md) or a JavaScript challenge when this limit is reached. If the captcha parameter is present and the set limit is reached, the user will be shown a CAPTCHA that they must validate to continue browsing.&#x20;

This validation will be valid for five minutes. So, if within the following five minutes after that validation, the user reaches the same limit again, the defined block time will be applied if specified. If the validation has expired and the user surpasses this threshold again, a new CAPTCHA will be shown.&#x20;

For example, if in our domain `www.my-site.com` we want to limit each user to a maximum of 20 requests per second, discriminating based on their IP address, and once that limit is reached, block the user for 30 seconds and require them to validate a CAPTCHA, we would deploy a [VCL](../../config/vcl/) [configuration](broken-reference) similar to the following:

```c
# rate limit with captcha
sub vcl_recv {
    if (req.http.host == "www.my-site.com") {
        set req.http.TCDN-WAF-Set-RateLimit-Key = req.http.True-Client-IP;
        set req.http.TCDN-WAF-Set-RateLimit-Options = "20:1s:30s:captcha";
        call rate_limit;
    }
}
```

In this way, once the set limit is reached, subsequent requests from the affected user will result in either a status code 429 (Too Many Requests) or a 418 (Robots are not allowed here!) if the CAPTCHA validation was incorrect.&#x20;

If the JavaScript challenge is selected, the first time that a request reaches the rate limit a non interactive JavaScript challenge will be sent to the user's browser to discern bots from humans.

```c
# rate limit with javascript challenge
sub vcl_recv {
    if (req.http.host == "www.my-site.com") {
        set req.http.TCDN-WAF-Set-RateLimit-Key = req.http.True-Client-IP;
        set req.http.TCDN-WAF-Set-RateLimit-Options = "20:1s:30s:js_challenge";
        call rate_limit;
    }
}
```

Obviously, these limits can be set for the entire site or for a specific part of it and can be discriminated based on different criteria, not just the user's IP address. For example, we could consider a usage quota of 30 requests per minute per API key.

```c
# rate limit
sub vcl_recv {
    if (req.http.host == "www.my-site.com") {
        set req.http.TCDN-WAF-Set-RateLimit-Key = "API key=" + req.http.API-Key;
        set req.http.TCDN-WAF-Set-RateLimit-Options = "30:60s";
        call rate_limit;
    }
}
```

O, perhaps, allow only a few POST or PUT requests per user:

```c
# rate limit
sub vcl_recv {
    if (req.http.host == "www.my-site.com") {
        if (req.method == "POST" || req.method == "PUT") {
            set req.http.TCDN-WAF-Set-RateLimit-Key = req.http.True-Client-IP;
            set req.http.TCDN-WAF-Set-RateLimit-Options = "2:10s";
            call rate_limit;
        }
    }
}
```

Obviously, these are just small examples of very specific use cases. If you have any questions about how to integrate this functionality into your own domain, please don't hesitate to contact us via email at [soporte@transparentcdn.com](mailto:soporte@transparetncdn.com).

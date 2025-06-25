# CAPTCHA

This function allows us to block requests made to our website by bots, ensuring that the traffic received comes from humans.&#x20;

With this purpose in mind, CAPTCHAs (Completely Automated Public Turing test to tell Computers and Humans Apart) consist of small automated tests or challenges that can differentiate between human users and automated programs (bots). Specifically, the implementation of this function is based on the [reCAPTCHA](https://www.google.com/recaptcha/about/) solution offered by Google.&#x20;

This function is invoked through our `TCDN-Command` header; therefore, we need to include the value `show-captcha.` The syntax of the function is as follows: `show-captcha[:`_`<ttl>`_`]`.&#x20;

Internally, when a user successfully completes the CAPTCHA challenge, a cookie called `TCDN-Captcha-UID` is assigned to their browser. This cookie allows for the unique identification of the user's session and prevents them from being repeatedly asked to pass the CAPTCHA challenge. However, this cookie has a finite time to live `(ttl)` of one hour. Once this time has elapsed, the user will need to complete a new CAPTCHA challenge. However, the optional parameter  _`<ttl>`_ allows us to control the lifetime of this session cookie.&#x20;

For example, if we wanted to limit the presence of bots on our domain `mi-dominio.es` without excessively inconveniencing legitimate users, we could implement a CAPTCHA with an 8-hour TTL. In this case, we would deploy a [VCL](../../config/vcl/) [configuration](broken-reference) similar to the following from the [dashboard:](../../getting-started/dashboard/)

```c
# show-captcha
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-Command = "show-captcha:28800s";
    }
}
```

In this way, if the user successfully completes the CAPTCHA challenge, they will be assigned the TCDN-Captcha-UID cookie and can continue browsing normally for the next eight hours, after which they will be presented with the CAPTCHA again. Alternatively, if the user is unable to solve the challenge, they will receive a 403 status code response (Robots are not allowed here!).

Of course, this is just a small example of a very specific use case. If you have any questions about how to integrate this functionality into your own domain, please don't hesitate to contact us at [soporte@transparentcdn.com](mailto:soporte@transparentcdn.com).

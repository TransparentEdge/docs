# CAPTCHA

This function allows us to block requests made to our website by bots, ensuring that the traffic received comes from humans.

With this purpose in mind, CAPTCHAs (Completely Automated Public Turing test to tell Computers and Humans Apart) consist of small automated tests or challenges that can differentiate between human users and automated programs (bots). Specifically, the implementation of this function is based on the [reCAPTCHA](https://www.google.com/recaptcha/about/) solution offered by Google.

This function is invoked through our `TCDN-Command` header; therefore, we need to include the value `show-captcha.` The syntax of the function is as follows: `show-captcha[:`_`<ttl>`_`]`.

Internally, when a user successfully completes the CAPTCHA challenge, the cookies `TEDGECP` and `TEDGECPS` (the same value, for cross-site iframes) are assigned to their browser. They are signed for the user's IP address and User-Agent, and prevent them from being repeatedly asked to pass the CAPTCHA challenge; if the IP address or the browser changes, the CAPTCHA is shown again. By default they have a time to live `(ttl)` of one hour. Once this time has elapsed, the user will need to complete a new CAPTCHA challenge. The optional parameter _`<ttl>`_ (up to 7 days) allows us to control the lifetime of these cookies.

For example, if we wanted to limit the presence of bots on our domain `mi-dominio.es` without excessively inconveniencing legitimate users, we could implement a CAPTCHA with an 8-hour TTL. In this case, we would deploy a [VCL](../../config/vcl/) [configuration](https://app.gitbook.com/s/dLlfX5hLpgfTCKcNCR8i/config) similar to the following from the [dashboard:](../../getting-started/dashboard/)

```c
# show-captcha
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-Command = "show-captcha:28800s";
    }
}
```

In this way, if the user successfully completes the CAPTCHA challenge, they will be assigned the `TEDGECP` cookies and can continue browsing normally for the next eight hours, after which they will be presented with the CAPTCHA again. Alternatively, if the user is unable to solve the challenge, the CAPTCHA page (403 status code) remains and asks them to try again.

The CAPTCHA is only shown on `GET` requests. Requests with other methods (`POST`, `HEAD`, ...) receive a `403` until the user has solved the CAPTCHA.

Of course, this is just a small example of a very specific use case. If you have any questions about how to integrate this functionality into your own domain, please don't hesitate to contact us at [soporte@transparentcdn.com](mailto:soporte@transparentcdn.com).

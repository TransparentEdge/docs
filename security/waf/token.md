# Content protected by token

This function allows us to protect content on our website by defining tokens with programmatically controlled validity periods.

## Theoretical explanation:

With this purpose in mind, the idea is that any request for a specific object (asset) on our website, such as a DASH (Dynamic Adaptive Streaming over HTTP) or HLS (HTTP Live Streaming) manifest, must include the values of three mandatory parameters:&#x20;

### Required Parameters:

1. Valid from (`vf`): The timestamp indicating when the token becomes valid.
2. Valid until (`vu`): The timestamp indicating when the token expires.
3. MD5 hash (`h`): The MD5 hash corresponding to the token. This hash is determined by the following pattern: `<vf>@<vu>@<secret>@<url excluding the mandatory token parameters (vf, vu, h)>.`

For example, if we want to protect the `/lista-reproduccion.m3u8?lang=es`, on our website `mi-dominio.com` with a token, the request should include the aforementioned mandatory parameters as follows: `/lista-reproduccion.m3u8?lang=es&vf=`_`<vf>`_`&vu=`_`<vu>`_`&h=`_`<h>`_.

Furthermore, the value of the parameter `h` will be the MD5 hash corresponding to the string _`<vf>`_`@`_`<vu>`_`@`_`<secret>`_`@/lista-reproduccion.m3u8?lang=es`. Similarly, _`<secret>`_&#x77;ill be the seed used in generating the token.

## How is it used?&#x20;

This function is invoked through our `TCDN-Command` header; therefore, we must include the value `protect-with-token` along with the set of parameters separated by a colon `(:)`. Specifically, the syntax of the function is as follows: `-with-token:secret=`_`<secret>`_`[:vf=`_`<valid from>`_`][:vu=`_`<valid until>`_`][:h=`_`<hash>`_`]`.&#x20;

_`<secret>`_ is the only mandatory parameter, which defines the seed used in token generation. It is the private key that underpins the entire process.&#x20;

Optionally, you can specify the parameters ,_`<valid from>`_, _`<valid until>`_ y _`<hash>`_ , and in a strict hard-coded manner within the [VCL](../../config/vcl/) [configuration](broken-reference). However, it is also possible to dynamically determine the value for these three parameters: either in the request itself through the query string, or by assigning corresponding cookies (`vf`, `vu`, and `h`).&#x20;

### Example of a token with a static validity period&#x20;

For example, if we want to protect the resource `/lista-reproduccion.m3u8?lang=es` on our domain `mi-dominio.es` using a token with a validity period of one year (e.g., 2022), we just need to deploy a similar [VCL](../../config/vcl/) [configuration](broken-reference) from the [dashboard:](../../getting-started/dashboard/)

```c
# protect-with-token
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/lista-reproduccion.m3u8\?lang=es") {
            set req.http.TCDN-Command = "protect-with-token:secret=ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM:vf=1640991600:vu=1672527599"; # desde '01/01/2022 00:00:00' hasta '12/31/2022 23:59:59'.
        }
    }
}
```

In the previous example, we can see that the string ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM has been defined as the value for the mandatory parameter _`<secret>`_. Additionally, values have been provided for two of the remaining three parameters, _`<valid until>`_ and _`<valid from>`_, with the respective values of `1640991600` (January 1, 2022) and 1672527599 (December 31, 2022).

```c
$ date --date='01/01/2022 00:00:00' +'%s'
1640991600
$ date --date='12/31/2022 23:59:59' +'%s'
1672527599
$
```

Therefore, the only remaining parameter is h, which corresponds to the MD5 hash of the string `1640991600@1672527599@ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM@/lista-reproduccion.m3u8?lang=es`, which is, `3caf5c965d2895f1705481d3a32d63b4`.

```c
$ echo -n "1640991600@1672527599@ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM@/lista-reproduccion.m3u8?lang=es" | md5sum | awk '{ print $1 }'
3caf5c965d2895f1705481d3a32d63b4
$
```

Therefore, requests to the resource `/lista-reproduccion.m3u8?lang=es` that are made without the h parameter or with an invalid h parameter will return a status code 401 (Unauthorized). If the requests are made before the start of the validity period, they will return a status code 404 (Not yet valid token). If the requests are made after the expiration of the token, they will return a status code 410 (Expired token).

Only requests to `/lista-reproduccion.m3u8?lang=es&h=3caf5c965d2895f1705481d3a32d63b4` or requests where a cookie named `h` with the correct value is defined will return the requested asset along with a status code 200.

### Example of a token with a dynamic validity period&#x20;

In the previous example, the validity period of the token is hard-coded in the configuration itself. If we want it to be dynamic, we can deploy a similar [VCL](../../config/vcl/) [configuration](broken-reference) from the [dashboard](../../getting-started/dashboard/):

```c
# protect-with-token
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.url ~ "^/lista-reproduccion.m3u8\?lang=es") {
            set req.http.TCDN-Command = "protect-with-token:secret=ESnrNc86j43DDwr3fAEpKm8zdBuUPZvmBmmZxAxZVQuQD7CN5LgJLD82hdzATjFM";
        }
    }
}
```

In this way, the parameters `vf` and `vu` should be provided either through the query string in the request or via corresponding cookies. Additionally, since `vf` and `vu` are now dynamic parameters, the value of `h` will also be dynamic as it depends on them.&#x20;

These are just small examples of a very specific use case. If you have any questions about integrating this functionality into your own domain, please don't hesitate to contact us at the email address:  [soporte@transparentcdn.com](mailto:soporte@transparetncdn.com).

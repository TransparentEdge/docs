---
description: Basic protection against hotlinking
---

# Avoiding Hotlinking

Although most browsers are evolving towards increased privacy by applying stricter default regarding the [Referrer-Policy](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Referrer-Policy), basic protection against [hotlinking](https://es.wikipedia.org/wiki/Hotlinking) can still be achieved.

To do this, you just need to define the `TCDN-Avoid-Hotlink-URL` header with the path to the resource you want to serve as a placeholder.&#x20;

For example, if you want to prevent hotlinking of images located in the `/wiki/content` path of your domain `www.example.com,` the VCL code to insert in the configuration would be similar to:

```c
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if(req.url ~ "^/wiki/contenido" && urlplus.get_extension() ~ "^(jpg|jpeg|png|gif|svg|mp4)$") {
            set req.http.TCDN-Avoid-Hotlink-URL = "/img/hotlink-placeholder.png";
        }
    }
}
```

As always, you can define it in a new `vcl_recv` block or within the existing one.&#x20;

Now, requests against those resources and under those conditions that have a referer different from the current site's domain will instead serve the placeholder `/img/hotlink-placeholder.png.` Defining a placeholder is mandatory.&#x20;

You can add any necessary conditions to the previous code. For example, if the domain `www.example2.com` is allowed to hotlink without any restrictions, the code would look like this:

```c
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if(
                req.url ~ "^/wiki/contenido" &&
                urlplus.get_extension() ~ "^(jpg|jpeg|png|gif|svg|mp4)$" &&
                req.http.referer !~ "^https?://www.example2.com"
          ) {
            set req.http.TCDN-Avoid-Hotlink-URL = "/img/hotlink-placeholder.png";
        }
    }
}c
```

{% hint style="warning" %}
By default, the following exceptions are included:

* Of course, if the referer matches the current domain, it does not apply.
* It is not correct to define `TCDN-Avoid-Hotlink-URL` with an empty string or a path that does not start with "/", in case of doing so, it will be replaced with "/".
* Some User-Agents are allowed by default (such as search engines and similar) to avoid harming the positioning.
* Empty or non-`HTTP(S)` protocol referers are excluded because it is impractical to do so without harming the website due to the Referrer Policy.
{% endhint %}

\

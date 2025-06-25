# TP-Cache

As we explain in the document

[https://docs.transparentedge.eu/v/english/getting-started/faq/arquitectura/capas-de-cache](https://docs.transparentedge.eu/v/english/getting-started/faq/arquitectura/capas-de-cache)

Transparent Edge is made up of two cache layers.

We can optionally add a header in the Mid-Tier to all the requests that pass through the CDN to let us know whether the object was served from the cache or whether it was necessary to go to the next layer or the origin server to find it.

These headers are **TP-Cache** (in layer 1) and **TP2-Cache** (only in Mid-Tier). This header can have the value **HIT** or **MISS** depending on whether or not the object was served from the cache.

Let's look at some examples:

```
TP-Cache: HIT
TP2-Cache: MISS
```

In this case, the object was served from one of the layer 1 caches, without having to go to the Mid-Tier to find it.

```
TP-Cache: MISS
TP2-Cache: HIT
```

This is the opposite scenario: The object was not in layer 1 or was expired there, and when requesting it from the Mid-Tier that cache found a fresh version of the object which was served from that layer. Therefore, the request did not ultimately have to go to the client's origin servers.

```
TP-Cache: MISS
TP2-Cache: MISS
```

In this final case, a valid copy of the object was not found in the cache, either because it was not a cacheable object or because it was expired, so the request went directly to the client's origin servers.

For reasons of clarity, in this document we have been speaking about the Cache-Control header, but Transparent Edge complies with the letter and spirit of the [RFC](https://www.rfc-editor.org/rfc/rfc2616#section-14), so any of the cache headers available there can be used.

```
curl -v -H "User-Agent: TCDN" https://www.transparentcdn.com/ > /dev/null
< HTTP/1.1 200 OK
< Server: nginx
< Date: Wed, 20 May 2020 15:11:57 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Link: <https://www.transparentcdn.com/wp-json/>; rel="https://api.w.org/"
< Link: </wp-includes/css/dist/block-library/style.min.css?ver=5.2.4>; rel=preload; as=style, </wp-content/plugins/contact-form-7/includes/css/styles.css?ver=5.1.4>; rel=preload; as=style, </wp-content/plugins/cookie-law-info/public/css/cookie-law-info-public.css?ver=1.8.1>; rel=preload; as=style, </wp-content/plugins/cookie-law-info/public/css/cookie-law-info-gdpr.css?ver=1.8.1>; rel=preload; as=style, </wp-content/plugins/revslider/public/assets/css/rs6.css?ver=6.1.1>; rel=preload; as=style, </wp-content/themes/cesis/style.css?ver=5.2.4>; rel=preload; as=style, </wp-content/themes/cesis_child_theme/style.css?ver=5.2.4>; rel=preload; as=style, </wp-content/themes/cesis/css/cesis_media_queries.css?ver=5.2.4>; rel=preload; as=style, </wp-content/themes/cesis/css/cesis_plugins.css?ver=5.2.4>; rel=preload; as=style, </wp-content/themes/cesis/includes/fonts/cesis_icons/cesis_icons.css?ver=5.2.4>; rel=preload; as=style, </wp-admin/admin-ajax.php?action=dynamic_css?ver=5.2.4>; rel=preload; as=style, </wp-content/plugins/js_composer/assets/css/vc_lte_ie9.min.css?ver=6.0.5>; rel=preload; as=style, </wp-content/plugins/js_composer/assets/css/js_composer.min.css?ver=6.0.5>; rel=preload; as=style, </wp-content/themes/cesis/admin/redux-extensions/extensions/dev_iconselect/dev_iconselect/include/fontawesome/css/font-awesome-social.css?ver=5.2.4>; rel=preload; as=style, </wp-includes/js/jquery/jquery.js?ver=1.12.4-wp>; rel=preload; as=script, </wp-includes/js/jquery/jquery-migrate.min.js?ver=1.4.1>; rel=preload; as=script, </wp-content/plugins/contact-form-7/includes/js/scripts.js?ver=5.1.4>; rel=preload; as=script, </wp-content/plugins/cookie-law-info/public/js/cookie-law-info-public.js?ver=1.8.1>; rel=preload; as=script, </wp-content/plugins/revslider/public/assets/js/revolution.tools.min.js?ver=6.0>; rel=preload; as=script, </wp-content/plugins/revslider/public/assets/js/rs6.min.js?ver=6.1.1>; rel=preload; as=script, </wp-content/themes/cesis/js/cesis_collapse.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/cesis_countup.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/cesis_easing.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/cesis_fittext.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/fitvids.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/fonticonpicker.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/lightgallery.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/owlcarousel.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/scrollmagic.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/cesis_transition.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/smartmenus.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/isotope.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/waypoints.js?ver=5.2.4>; rel=preload; as=script, </wp-content/themes/cesis/js/cesis_custom.js?ver=5.2.4>; rel=preload; as=script
< TP-l2-Cache: HIT
< X-Device: desktop
< Age: 5622
< content-security-policy-report-only: default-src https: data: 'unsafe-inline' 'unsafe-eval'; report-uri https://api.transparentcdn.com/v1/csp/report/4/
< TP-Cache: HIT
< Vary: , Accept-Encoding
< strict-transport-security: max-age=31536000; includeSubDomains; preload
```

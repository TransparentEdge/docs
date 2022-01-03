# TP-Cache

Como explicamos en el documento [https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura\_de\_caches?parent=Wiki](https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura\_de\_caches?parent=Wiki),&#x20;





Transparent Edge Services se compone de dos capas de caché.&#x20;

En todas las peticiones que pasan por Edge Distributed Content y de manera opcional, por el midtier, introducimos una cabecera que nos permite saber si el objeto ha sido servido desde la caché o si, por el contrario, ha tenido que ir a buscarlo a la siguiente capa u origen.\
Estas cabeceras son **TP-Cache** (en capa 1) y **TP2-Cache** (solo en midtier). Esta cabecera puede tener el valor **HIT** o **MISS** en función, como decíamos, de si el objeto ha sido o no servido desde la caché.

Vamos a poner varios ejemplos:

```
TP-Cache: HIT
TP2-Cache: MISS 
```

En este caso, el objeto ha sido servido desde una de las cachés de capa 1, no habiendo sido necesario ir a buscar dicho objeto al midtier.

```
TP-Cache: MISS
TP2-Cache: HIT
```

Este caso es justo el caso contrario: el objeto no estaba o estaba caducado en capa 1 y, al pedirlo al midtier, esta caché ha encontrado una versión fresca del objeto, que se ha servido desde esta capa. Por ello, la petición no ha tenido que ser pedida en última instancia a los servidores de origen del cliente.

```
TP-Cache: MISS
TP2-Cache: MISS
```

En este último caso no se ha encontrado una copia del objeto válida en caché, bien porque era un objeto no cacheable o bien porque estaba caducado, por lo que la petición ha bajado directamente a los servidores de origen del cliente.

En este documento, por claridad, hemos hablado de la cabecera Cache-Control, pero Transparent Edge Services cumple con el estándar y se ciñe a lo que reza la [RFC](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html), por lo que puede utilizarse cualquiera de las cabeceras de cacheé disponibles en la misma.

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


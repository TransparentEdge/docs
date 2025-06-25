# Log formats

Transparent Edge makes its website logs available to customers. This can work in two different ways:

1\. (PULL): The customer can download the logs from an FTP set up for that purpose.

2\. (PUSH): Transparent Edge can send the logs every hour to an FTP that the customer makes available to us for that purpose.

This service is not enabled by default. If you’d like to enable it, follow the instructions[ here](https://docs.transparentedge.eu/getting-started/dashboard/envio-de-logs).

## Log format - Web Caching Service

```
203.0.113.5 - - [10/Aug/2024:06:20:13 +0000] "GET http://www.transparentedge.eu/wp-content/plugins/ninja-forms/assets/fonts/fontawesome-webfont.woff2 HTTP/2.0" 200 66624 "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36" hit "font/woff2" "L1" 1194 "82" "https://www.transparentedge.eu/wp-content/cache/wpo-minify/1724664512/assets/wpo-minify-footer-a97725b0.min.css" https ES "OK" "TCDN-OK:782763138" 1766
```

* **Field 1**: Client IP.
* **Field 2**: Remote logname. Always '-'.
* **Field 3**: Username from auth basic.
* **Field 4**: Date and time (UTC).
* **Field 5**: HTTP Method.
* **Field 6**: URL requested.
* **Field 7**: Protocol.
* **Field 8**: Status code.
* **Field 9**: Size of the object.
* **Field 10**: User-Agent.
* **Field 11**: Result of the request (HIT/MISS).
* **Field 12**: Content type.
* **Field 13**: Cache layer that served the request (L1/L2).
* **Field 14**: Response time of the request in microseconds.
* **Field 15**: Client ID.
* **Field 16**: Referer.
* **Field 17**: Real protocol (because we undo the HTTPS in a layer prior to the cache layer, this field is necessary to identify if the original request was made via HTTP o HTTPS).
* **Field 18**: Country code.
* **Field 19**: Multi-purpose field. If [BotM](../../security/bot-mitigation.md) is active and performs any action, it's shown here.
* **Field 20**: [Origin Error.](http-5xx-error-codes.md)
* **Field 21**: Remaining TTL of the object.



## Format of WAF logs

```
47.62.117.136 - - [25/May/2020:15:03:22 +0000] "GET http://www.example.com/elt/ HTTP/1.0" 200 12423 "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36" "text/html" "waf" 1 "179"
```

* **Field 1**: Client IP.
* **Field 2**: Remote logname. Always '-'.
* **Field 3**: Username from auth basic.
* **Field 4**: Date and time + time difference.
* **Field 5**: Host header.
* **Field 6**: Request URL.
* **Field 7**: Protocol.
* **Field 8**: Status code.
* **Field 9**: Size of the object.
* **Field 10**: User-Agent.
* **Field 11**: Content type.
* **Field 12**: Layer that served the request (in this case WAF).
* **Field 13**: Response time of the request.
* **Field 14**: Client ID.

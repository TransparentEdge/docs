---
description: Learn the meaning of the debug strings
---

# Debug codes

Every response served by Transparent Edge carries a debug string that tells you, at a glance, what happened to that request. You will find the same string in two places:

* In the **`reason` response header** of the request itself.
* In the **analytics section of the dashboard**, for every logged request.

A string has two parts separated by a colon: the reason and the request ID.

```
reason: TCDN-BENG-Origin Timeout:4123fd06e0-1795161458-224112845
        └──────── reason ───────┘└───────── request ID ────────┘
```

The reason is the part that tells you what happened. The request ID identifies that single request in our logs: you can ignore it day to day, but **quote it when you open a support ticket**, because it is what lets us find the exact request you are asking about.

### Requests served normally

| Reason          | Explanation                                                                                                                                                                                                                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TCDN-OK`       | Everything went fine. This should be the most common value by far.                                                                                                                                                                                                                                                            |
| `TCDN-OK-STALE` | The request was served from a cached copy that had already expired. This normally means your origin was failing and the CDN served the previous version rather than an error page. See [Protection against origin failures](https://docs.transparentedge.eu/getting-started/faq/features/protection-against-origin-failures). |

### Requests resolved at the edge

These never reached your origin: the edge node produced the response itself.

<table><thead><tr><th width="196">Reason</th><th>Explanation</th></tr></thead><tbody><tr><td><code>TCDN-RPE-XXX</code></td><td>RPE is for <code>Request Processed at Edge</code>. <code>XXX</code> is the status code the VCL configuration used to answer: a <code>deny_request</code>, a redirect, and so on. These requests appear in the CDN logs only, since your origin was never involved.</td></tr><tr><td><code>TCDN-RPE-GRL-429-Too Many Requests</code></td><td>The global rate limit stopped the request. It is there to protect your origin from sudden spikes. If your site receives legitimate spikes, contact us and we will adjust the limit.</td></tr><tr><td><code>TCDN-RPE-Under Attack</code></td><td>The request was held by "Under Attack" mode. A legitimate visitor using a normal browser is checked in a moment and then sent on to the page they asked for.</td></tr><tr><td><code>TCDN-RPE-UA-429-Too Many Requests</code></td><td>"Under Attack" mode stopped this request several times in a row, normally because it arrived far more often than a regular visitor would ask. It is usually a script or a tool making too many requests.</td></tr><tr><td><code>TCDN-RPE-UA-403-Denylist</code></td><td>"Under Attack" mode is enabled and this IP address is on the deny list, because it took part in previous attacks.</td></tr></tbody></table>

### Backend errors

Something went wrong between the CDN and your origin. Each of these is explained in detail, with the HTTP status code returned and what to check first, in [HTTP 5xx Error Codes](https://docs.transparentedge.eu/getting-started/faq/http-5xx-error-codes).

| Reason                         | Explanation                                                                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TCDN-BENG-Origin Timeout`     | Your origin did not answer in time. Returned as **504**.                                                                                                                  |
| `TCDN-BENG-Origin Unreachable` | The connection to your origin failed immediately. Returned as **503**.                                                                                                    |
| `TCDN-BENG-Sick Healthcheck`   | Your origin is marked as unhealthy by its health check. Returned as **503**.                                                                                              |
| `TCDN-BENG-XXX`                | Your origin answered with an error. `XXX` is the status code it returned. The error is in your application and has to be fixed there.                                     |
| `TCDN-SBNG`                    | The request to your origin failed and could not be attributed to any of the causes above.                                                                                 |
| `TCDN-SBPR`                    | A non-cacheable request — POST, PUT, PATCH or DELETE — produced an error at your origin. These methods are never cached, so there is no previous version to fall back on. |
| `TCDN-OENG-XXX`                | Your origin returned an error and it was passed through to the visitor unchanged, because `X-Show-Origin-Errors` is enabled for your service.                             |
| `TCDN-DNF`                     | Domain Not Found: the domain points to Transparent Edge, but no configuration assigns a backend to that hostname. Returned as **520**.                                    |
| `TCDN-NBD`                     | No Backend Defined: the domain exists in the CDN but has no backend assigned. Returned as **520**.                                                                        |

### Anything else

| Reason       | Explanation                                                                             |
| ------------ | --------------------------------------------------------------------------------------- |
| `TCDN-E-XXX` | An error we have not classified. Contact us with the request ID and we will look it up. |

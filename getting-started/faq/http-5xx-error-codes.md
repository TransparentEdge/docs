# HTTP 5xx Error Codes

When a[ web server](https://en.wikipedia.org/wiki/Web_server) is not able to process a request or simply isn’t available, your web browser receives a 5xx error response code—or in other words a code between 500 and 599.

These are web server error codes, which are very different from other errors like 404 - Not found, which typically occurs when you request an object that doesn’t exist on the server.

Some of the most frequent error codes are:

* 500 - Internal Server Error
* 502 - Bad Gateway
* 503 - Service Unavailable
* 504 - Gateway Timeout

When the error happens between the CDN and your origin, Transparent Edge serves its own error page and reports **what went wrong in the `reason` response header**. The value of that header starts with a `TCDN-` identifier and ends with the request ID:

```
reason: TCDN-BENG-Origin Timeout:4123fd06e0-1795161458-224112845
```

The request ID is the part after the last colon. **Quote it when you contact support**: it is what lets us find that exact request in our logs.

### Status codes we return

The status code tells the browser, and any monitoring you have in front, what kind of problem it is:

| Status  | When                                                                                               |
| ------- | -------------------------------------------------------------------------------------------------- |
| **504** | The origin did not answer in time                                                                  |
| **503** | Any other backend error: unreachable origin, failing health check, or a 5xx returned by the origin |
| **520** | The domain is not registered in the CDN, or has no backend assigned                                |

If you enable the `X-Show-Origin-Errors` header for your service, 5xx responses produced _by your origin_ keep their original status code instead (see below).

If the object was cacheable and the CDN still had a copy of it, you will not see any of these: the\
previous version is served instead, reported as `TCDN-OK-STALE`. See\
[Protection against origin failures.](https://docs.transparentedge.eu/getting-started/faq/features/protection-against-origin-failures)

## List of codes

#### Backend errors

| Identifier                     | Status | Meaning                                                                                                                                                               |
| ------------------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TCDN-BENG-Origin Timeout`     | 504    | The CDN reached your origin, or tried to, and gave up waiting. Either the connection did not complete, or the origin accepted it and did not send a response in time. |
| `TCDN-BENG-Origin Unreachable` | 503    | The connection to your origin failed immediately: connection refused, DNS resolution failure, TLS handshake failure, or no route to the host.                         |
| `TCDN-BENG-Sick Healthcheck`   | 503    | Your origin is marked as unhealthy because its health check is failing, so the CDN did not send the request to it.                                                    |
| `TCDN-BENG-<5xx>`              | 503    | Your origin answered, and answered with an error. The number is the status code it returned, for example `TCDN-BENG-500`.                                             |
| `TCDN-SBNG`                    | 503    | The request to the origin failed and could not be attributed to any of the causes above.                                                                              |
| `TCDN-SBPR`                    | 503    | A non-cacheable request (POST, PUT, PATCH or DELETE) produced an error at the origin.                                                                                 |
| `TCDN-OENG-<5xx>`              | 5xx    | Your origin returned an error and it was passed through to the visitor unchanged, because `X-Show-Origin-Errors` is enabled for your service.                         |

#### Configuration errors

| Identifier | Status | Meaning                                                                              |
| ---------- | ------ | ------------------------------------------------------------------------------------ |
| `TCDN-DNF` | 520    | Domain Not Found: the domain is not registered in the CDN.                           |
| `TCDN-NBD` | 520    | No Backend Defined: the domain exists in the CDN, but has no backend assigned to it. |

### Showing your origin's own errors

By default, a 5xx returned by your origin is replaced with the Transparent Edge error page, and the status is normalised to 503. If you would rather have your visitors see exactly what your origin answered — its own status code, its own error page — ask support to enable the `X-Show-Origin-Errors` header for your service. Those responses are then reported as `TCDN-OENG-<status>`.

This only applies to errors your origin actually answered. If the CDN could not reach your origin at all, there is nothing to pass through, and you get the Transparent Edge error page with the corresponding identifier from the table above.

### What to look at first

| What you see            | Where to look                                                                                                                                                                    |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Origin Timeout`        | Your origin is slow or saturated. Check its own logs for the same request ID window, and check whether the timeouts configured for your backend are realistic for that endpoint. |
| `Origin Unreachable`    | Your origin refused the connection or did not resolve. Check that it is listening, that DNS is correct, and that the CDN's address ranges are allowed through your firewall.     |
| `Sick Healthcheck`      | The health check configured for your backend is failing. Check the URL it requests and what your origin answers to it.                                                           |
| `TCDN-BENG-<5xx>`       | The error is inside your application: the CDN only relayed it. The status code in the identifier is the one your origin returned.                                                |
| `TCDN-DNF` / `TCDN-NBD` | Configuration: the domain or its backend is missing in the control panel.                                                                                                        |

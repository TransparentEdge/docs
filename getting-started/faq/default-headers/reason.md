# reason

Every response served by Transparent Edge carries a `reason` header with a debug string describing what happened to that request:

```
reason: TCDN-OK:4123fd06e0-1795161458-224112845
reason: TCDN-BENG-Origin Timeout:4123fd06e0-1795161458-224112846
```

It has two parts separated by a colon: the reason and the request ID. On a normal request the reason is `TCDN-OK`; when something goes wrong it names the cause.

The same string is recorded for every request in the analytics section of the dashboard, so you can look for a pattern there as well as inspecting a single response.

**When you open a support ticket, include the full value of this header.** The request ID is what lets us find that exact request in our logs.

* The complete list of strings: [Debug codes](../../../guides/debug-codes.md)
* The ones that mean an error between the CDN and your origin: [HTTP 5xx Error Codes](../http-5xx-error-codes.md)

# HTTP 5xx Error Codes

When a[ web server](https://en.wikipedia.org/wiki/Web_server) is not able to process a request or simply isn’t available, your web browser receives a 5xx error response code—or in other words a code between 500 and 599.

These are web server error codes, which are very different from other errors like 404 - Not found, which typically occurs when you request an object that doesn’t exist on the server.

Some of the most frequent error codes are:

* 500 - Internal Server Error
* 502 - Bad Gateway
* 503 - Service Unavailable
* 504 - Gateway Timeout

For technical reasons, CDNs mask all those status codes into a single error code. At Transparent Edge, we use the code 503 for any 5xx server error.

There can be many reasons for a 503 error from the CDN. They are categorized in the response headers and in the HTML code of the error, providing relevant information that can help us discern the error type. These headers begin with TCDN- and all of them end with a numerical value that lets us accurately identify the request. For instance:

```
TCDN-BENG-504:275330054.
```

## List of codes

* **`TCDN-BENG-5xx`** - Backend Error No Grace

The object isn’t in the cache and the origin has responded with a **`5xx`** error not categorized in any of the tags below. The number in the header is the exact error code returned by the origin. For instance: `TCDN-BENG-504`.

* **`TCDN-BENG-Service Unavailable`** - Backend Error No Grace (Network error)

The object isn’t in the cache and a network error ocurred while fetching it from the origin backend.

* **`TCDN-SBPR`** - Sick Backend POST/PUT Request

The request is not cacheable and caused an error at the origin.

* **`TCDN-SBNG`** - Sick Backend No Grace

The origin is marked as sick (its healthcheck is failing) and the object isn’t in the cache.

* **`TCDN-DNF`** - Domain Not Found

The domain was not found; it is not registered in the CDN.

* **`TCDN-NBD`** - No Backend Defined

The domain exists in the CDN but no backend has been assigned to it.

* **`TCDN-OENG`** - _Origin Error No Grace_&#x20;

> There was an origin error 5xx and we are keeping the original status code from origin without 503 status code masking due the **`X-Show-Origin-Errors`**&#x68;eader.

* **`TCDN-OK`** - No backend error found

The request was delivered successfully.

---
description: >-
  How long the CDN waits for your origin, and what happens when it does not
  answer
---

# Origin timeouts

When an object is not in cache, the CDN has to fetch it from your origin. Three timeouts govern how long it is willing to wait at each stage of that fetch:

| Timeout           | Default | Starts                                         | Ends                                        |
| ----------------- | ------- | ---------------------------------------------- | ------------------------------------------- |
| **Connect**       | 5s      | When the CDN starts opening the TCP connection | When the connection is established          |
| **First byte**    | 20s     | When the request has been sent                 | When the first byte of the response arrives |
| **Between bytes** | 5s      | After each chunk of the response               | When the next chunk arrives                 |

The first two cover a request that has not produced anything yet. The third one covers a response already in progress: it limits the gap between two chunks, not the total duration. A download of any length is fine as long as data keeps flowing.

### What happens when one is exceeded

The fetch is abandoned and the visitor receives an error page with status **504** and `TCDN-BENG-Origin Timeout` in the `reason` header.

With one exception, and it is the common case for a well configured site: if the object is cacheable and the CDN still holds a copy of it, that copy is served instead of the error, reported as `TCDN-OK-STALE`. See [Protection against origin failures](protection-against-origin-failures.md).

### Choosing values

The defaults suit a normal web origin. They are worth revisiting when:

* **An endpoint is legitimately slow.** A report, a search, an export or a PDF that takes 40 seconds to generate will be cut off at 20 seconds with the default first byte timeout. This is the one to raise, and only for those paths.
* **You serve long responses with pauses in them.** Streaming, a slow export written row by row, a large file produced on the fly. Here the one that matters is between bytes: if your origin stops for more than 5 seconds in the middle of sending, the response is cut even though it had already started well.
* **Your origin is behind a queue or a cold start.** A connection that is accepted but then parked for a while consumes first byte time, not connect time.

Raising a timeout is not free: while the CDN waits, the visitor waits too, and a request that is going to fail takes longer to say so. Raise the one that matches the problem, not all of them.

The maximum accepted value for any of these is 600 seconds.

### Changing them for a backend

The values are set per backend and are not editable from the dashboard. Contact support indicating which backend and which value you need, and why — it helps us tell a slow endpoint from an origin problem worth looking into.

### Changing them for specific requests

If you provision your own VCL, you can override any of them per request from `vcl_backend_fetch`, which is useful when only part of your site needs a different value:

```vcl
sub vcl_backend_fetch {
    # Report generation can take up to two minutes
    if (bereq.url ~ "^/admin/reports/") {
        set bereq.http.TCDN-First-Byte-Timeout = "120s";
    }
}
```

| Header                       | Overrides             |
| ---------------------------- | --------------------- |
| `TCDN-Connect-Timeout`       | Connect timeout       |
| `TCDN-First-Byte-Timeout`    | First byte timeout    |
| `TCDN-Between-Bytes-Timeout` | Between bytes timeout |

The value is a duration with its unit, for example `30s` or `2m`. A header set this way takes precedence over the value configured for the backend, and values above 600 seconds are capped.

### If your service uses two cache layers

When your service is configured with the second cache layer, the timeouts you configure are applied by the layer that actually talks to your origin, so the behaviour and the values are the same as with a single layer.

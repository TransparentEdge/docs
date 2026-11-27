# Protection against origin failures

When there are connectivity problems between Transparent Edge’s servers and the client's origin servers—whether due to a service failure or any other problem recovering content from the origin—Transparent Edge will serve an expired version of the content. This will be done for up to a maximum of eight hours or until communication is reestablished between Transparent Edge and the client’s origin servers.

For Transparent Edge to be able to serve that expired content when there are problems, the objects served need to have a cache time (Cache-Control: **max-age**) of over 1 second. In other words, objects marked with non-cacheable headers (No-Store, No-Cache, Private, etc.) will not be served when there are problems, because they don't exist in the cache. So these requests will return an error page: **504** if the origin timed out, **503** otherwise. See [HTTP 5xx Error Codes](../http-5xx-error-codes.md).

### How to tell when this is happening

When the CDN serves an expired copy because your origin was failing, the response carries `TCDN-OK-STALE` in its `reason` header, and the request is logged with that same string in the dashboard. If you ever see content that looks older than you expect, that is the string to look for: it means your origin was not answering and the CDN covered for it.

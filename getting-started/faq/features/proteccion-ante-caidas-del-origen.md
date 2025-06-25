# Protection against origin failures

When there are connectivity problems between Transparent Edge’s servers and the client's origin servers—whether due to a service failure or any other problem recovering content from the origin—Transparent Edge will serve an expired version of the content. This will be done for up to a maximum of eight hours or until communication is reestablished between Transparent Edge and the client’s origin servers.

For Transparent Edge to be able to serve that expired content when there are problems, the objects served need to have a cache time (Cache-Control: **max-age**) of over 1 second. In other words, objects marked with non-cacheable headers (No-Store, No-Cache, Private, etc.) will not be served when there are problems, because they don't exist in the cache. So these requests will return an HTTP 503 error.

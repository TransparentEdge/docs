# Status Code

A status code is an HTTP or HTTPS response code used by the server to let the client know the status of a request and whether or not it was successful.

The[ RFC 2616, section 10](https://www.rfc-editor.org/rfc/rfc2616) lists all the status codes supported by HTTP 1.1, but we are going to highlight the most relevant ones here.

## Successful response

**200 OK**

The request was successful and the server returns a 200 status code to let the client know.[![Edit](https://soporte.transparentcdn.com/images/edit.png)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Status_code/edit?section=4)

**206 Partial Content**

The server is only delivering part of the content. This type of response is given to range requests when the client explicitly requests part of an object using the Content-Range header.

## Redirections

**301Moved Permanently**

This status code indicates a permanent redirection. This type of request can be cached by proxy cache and by browsers. They must be used carefully because they can cause confusion.

**302 Found**

This is used to identify a temporary redirection, so it’s never cached in proxies or browsers.

**304 Not Modified**

This indicates that the content has not been modified on the server. It is sent as a response to requests from browsers and proxies to client web servers with the If-Modified-Since header.

## Client errors

**400 Bad Request**

We see this type of error when the server can't process a request sent by the client. It’s not generally seen in browsers and is more often seen when processes or applications make their own HTTP requests. It’s usually due to a poorly formed request.

**401 Unauthorized**

Sometimes a website or part of it is protected under a username/password and this status code warns us of unauthorized access. Requests with authentication are not cached in Transparent Edge.

**403 Forbbiden**

This tells us we are entering a restricted part of the website and our access is denied, possibly for security reasons. It may be due to an IP restriction or because a prohibited action is being attempted to exploit a vulnerability of the web application. In Transparent Edge, the latter only occurs if you have the Secure Layer.

**404 Not Found**

This type of error is very common. It simply means that the web resource we are requesting cannot be found on the server.

**405 Method not Allowed**

This status code is returned when the client makes an HTTP request using a protocol not supported by the cache or server. Transparent Edge allows the following methods:

```
GET
POST
PUT
DELETE
OPTIONS
PROPFIND
PUSH
HEAD
```

**408 Timeout**

This is a complicated error. It means that the connection has been closed due to exceeding the wait time. If the error occurs on the client server, it will become a 503 in Transparent Edge. Under normal conditions, it’s often due to a communication problem: either an internal firewall, an internal server error, or a firewall/proxy between Transparent Edge and the client. Any “connection close” or “closing connection” message in the client’s server logs could mean a 408. Historically, we’ve had problems with Apache modules that cause this error, like the mpm\_itk ([http://mpm-itk.sesse.net/](http://mpm-itk.sesse.net/)).

## Errores de servidor

**500 Internal Server Error**

This indicates a server error, generally with the application. For instance, it can occur when there are unclosed quotation marks in PHP.

**502 Bad Gateway**

This is a code returned by some web servers when acting as a reverse proxy and the resource behind them is not available. It’s very common with Nginx.

**503 Service Unavailable**

The service is not available. This error is returned when there’s no contact with the client’s web server, either because it’s down or due to a communication problem. It's generated “on the fly” by Transparent Edge when the client has a fatal error. When 503 error messages are received by the client, look for errors in its logs.

**504 Gateway Timeout**

This response code typically occurs when the client’s origin server is taking too long to return a non-cached object, either because it should not be cached or because it has expired.

Transparent Edge protects you against all 5XX errors by showing you the latest cached version. You can get more information [here](https://docs.transparentedge.eu/v/english/getting-started/faq/funcionalidades/proteccion-ante-caidas-del-origen).

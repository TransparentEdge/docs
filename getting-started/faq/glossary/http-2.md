# HTTP/2

HTTP/2 is a major revision of the HTTP protocol that enables more efficient use of web resources. It is compatible with HTTP/1.1 and does not involve significant changes to basic concepts like HTTP methods, header fields, and status codes.

It does however include countless improvements, like the use of a single connection, the compression of header fields, and the server push or cache push service that enables the server to preemptively push information to the client before the client requests it. So, for instance, when an HTML page is requested that is linked to style sheets, images, and other resources, everything is sent together in the first request (multiplexing) rather than having to wait to parse the HTML file.

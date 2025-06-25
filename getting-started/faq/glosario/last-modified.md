# Last-Modified

The Last-modified header is an HTTP response header that contains the date and time when the origin server believes the resource was last modified.

It is used as a validator to determine if the resource is the same as the previously stored one. It is less accurate than an ETag header, but it can be a fallback mechanism.

Conditional requests containing If-Modified-Since or If-Unmodified-Since headers make use of this field.

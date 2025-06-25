# Configure your servers to send cache headers

We will explain how to configure servers to send cache headers in **Apache** and **Nginx.**

### Apache

To configure cache headers in Apache, we can use two modules: _mod\_expires_ and _mod\_header._

#### mod\_expires

This module allows us to manipulate the HTTP Expires header and the server's max-age directive. We can set the expiration date for cached resources. This module can be applied in the following contexts: server config, virtual host, directory, .htaccess.

**Configuration example:**

```
<ifModule mod_expires.c>
ExpiresActive On
ExpiresDefault "access plus 5 seconds" 
ExpiresByType image/x-icon "access plus 2592000 seconds" 
ExpiresByType image/jpeg "access plus 2592000 seconds" 
ExpiresByType image/png "access plus 2592000 seconds" 
ExpiresByType image/gif "access plus 2592000 seconds" 
ExpiresByType text/javascript "access plus 216000 seconds" 
ExpiresByType application/javascript "access plus 216000 seconds" 
ExpiresByType text/html "access plus 600 seconds" 
ExpiresByType application/xhtml+xml "access plus 600 seconds" 
</ifModule>
```

For more information about mod\_expires, click [here](https://httpd.apache.org/docs/2.4/mod/mod_expires.html).

#### mod\_headers

It provides directives that allow us to have control over the response headers. We can replace, merge, or remove the headers of our website. This module can be applied in the following contexts: server config, virtual host, directory, and .htaccess.

**Configuration example:**

```
 # BEGIN Cache-Control Headers
<ifModule mod_headers.c>
  <filesMatch "\.(ico|jpe?g|png|gif|swf)$">
    Header set Cache-Control "public" 
  </filesMatch>
  <filesMatch "\.(css)$">
    Header set Cache-Control "public" 
  </filesMatch>
  <filesMatch "\.(js)$">
    Header set Cache-Control "private" 
  </filesMatch>
</ifModule>
# END Cache-Control Headers
```

For more information about mod\_expires, click [here.](https://httpd.apache.org/docs/2.2/mod/mod_headers.html)

### Ngninx

To configure cache headers in Nginx, we will use the **ngx\_http\_headers\_module.**

This module allows us to add "Expires" and "Cache-Control" directives to a response header. We can apply it in the following contexts: http, server, location, and if in location.

**Configuration example:**

```
expires    24h;
expires    modified +24h;
expires    @24h;
expires    0;
expires    -1;
expires    epoch;
expires    $expires;
add_header Cache-Control private;
```

# Query String

The query string is the part of a URL that contains the data to be sent to the web application. It generally includes fields added to the base URL by the client application, for example as part of an HTML form.

```
www.ejemplo.net/web.php?campo1=valor1&campo2=valor2
```

The example above is for a dynamic website. In this case, the server automatically creates the response page when the browser requests it, using the data that appears in the URL.

There are other methods of generating dynamic pages, such as separating the query strings using the “/” character. The server configuration lets us generate friendly URLs, like the one below:

```
www.ejemplo.net/main/blog/132
```

In this case, the name of the variable is predefined on the server based on the variable’s position in the URL, and the content of the variable is its value (main, blog, 132).

# Enrutando el tráfico a distintos backend

En Transparent Edge Services es posible tener una configuración "**multiorigen**", de manera que se pueda recuperar los objetos de diferentes backend en función de cualquier elemento que esté presente en la petición que el navegador hacer a Transparent Edge Services.

Un caso muy típico podría ser el querer cambiar de origen en función de la url, de manera que, como ponemos en el ejemplo, todo lo que llegue a https://www.transparentcdn.com/blog vaya al backend c83\_tcdn\_blog, previamente dado de [alta](../getting-started/dashboard/autoprovisionamiento/sites-and-backends.md).

```javascript
sub vcl_recv{
  if (req.http.host == "www.transparentcdn..com"){ 
    set req.backend_hint = c83_tcdn.backend();
  } 
  if ((req.http.host == "www.transparentcdn.com") && (bereq.url ~ "/blog")) {
    set req.backend_hint = c83_tcdn_blog.backend();
  }
}
```

Esta misma lógica que presentamos aquí puede usarse para cambiar de backend en función, como decimos, de cualquier otro elemento presente en la request, como una cookie o una cabecera cualquiera.

# ESI Tags

Edge side Include (ESI) es un lenguaje de marcas que desarrollo Akamai y propuso como estándar a la w3c pero que en la actualidad continua siendo un borrador. Aun así Transparent Edge Services da soporte a algunas de las etiquetas ESI disponibles.

ESI consiste en embeber etiquetas ESI dentro del html de manera que esas llamadas se realizan de manera independiente como peticiones en paralelo.

```
<HTML>
<BODY>
The time is: <esi:include src="/cgi-bin/date.cgi"/>
at this very moment.
</BODY>
</HTML>
```

En el ejemplo de arriba, que podría ser el index.html de una página, con su tiempo de cache de por ejemplo 3600s (1h) estamos haciendo una include dentro del html principal que se tratará como una llamada en paralelo con su tiempo de cache independiente al de la página principal. De manera que la llamada al date.cgi no tenga cache y el resto de la página si lo tenga.

Como hemos mencionado anteriormente, Transparent Edge Services soporta solo los tags \<esi:include> y \<esi:remove>

```
<esi:remove>
  <a href="http://soporte.transparentcdn.com"> Enlace alternativo cuando ESI no funcione</a>
</esi:remove>
```

Está funcionalidad viene por defecto en Transparent Edge Services con el precio del servicio, pero hay que activarla para cada site.

Para ello, nos vamos al [portal](https://dashboard.transparentcdn.com) y pinchamos sobre la pestaña de **Provisioning** **- VCL Configs** y nos vamos al modo avanzado y duplicamos la última configuración activa añadiendo un código similar a este pero adaptando el nombre de tu dominio:

```javascript
sub vcl_backend_response {
        if (bereq.http.host ~ ".*.transparentcdn.com") {
                set beresp.do_esi = true;
                unset beresp.http.ETag;
                unset beresp.http.Last-Modified;
        }
}
```

&#x20;Una vez hecho esto y cuando la nueva configuración esté desplegada, ya podrás servir ESI desde Transparent Edge Services.

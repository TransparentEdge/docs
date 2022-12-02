# Redirecciones

#### Redirección a HTTPS

Cuando tu _site_ funciona con SSL, es muy común que quieras que todo lo que llegue por HTTP sin encriptar se envíe a su versión segura para tener la certeza de que nadie navega por tu sitio web de forma no segura. Para ello basta con _setear_ la cabecera **req.http.Command** con el valor "**redirect\_https**".&#x20;

```javascript
 sub vcl_recv {  
     if ( req.http.host == "www.transparentedge.eu") {
         set req.http.TCDN-Command = "redirect_https, " + req.http.TCDN-Command;
     }
}
```

Visita este [enlace](../../../config/vcl/tcdn-command.md) para entender el correcto funcionamiento de la cabecera TCDN-Command.

#### Redirecciones 301 y 302

Hay ocasiones en las que es necesario que tu contenido sea enviado a otra parte de tu _site_ o a una URL externa. En esos casos podemos hacer una redirección temporal (302) o una redirección permanente (301).&#x20;

Para ello, debemos definir la condición en la función _vcl\_recv_ y hacer un _set_ de la cabecera _Req.Http.X-RetSynth_, la cual espera dos parametros:

1. **Tipo de redirección:** 751 (para hacer un 301) o 752 (para hacer un 302).
2. **URL de destino:** destino del _redirect_ con el protocolo deseado.

```javascript
# Redirect 302
sub vcl_recv{
   if (! req.http.User-Agent ~ "curl" ){
      set req.http.X-retSynth = "752,https://www.transparentedge.eu/";
    }
}

# Redirect 301
sub vcl_recv{
   if (! req.http.User-Agent ~ "curl" ){
      set req.http.X-retSynth = "751,https://www.transparentedge.eu/";
    }
}
```

Es importante conocer las implicaciones del tipo de redirección que elijas. Un 301 es una redirección que, teóricamente, no va a cambiar y, por tanto, se cachea en los navegadores. Si luego cambia, es muy complicado descachear ese recurso.&#x20;

Un 302, por su parte, es un _redirect_ que haces sobre un recurso de forma temporal y, por tanto, no se cachea en los navegadores ni en los _proxys_ intermedios.

# A/B Testing

### ¿Qué es A/B Testing y por qué querríamos implementarlo?

Mediante _A/B testing_ podemos exponer a los usuarios a dos experiencias ligeramente distintas, algo tan simple como cambiar el color de un botón en la inferfaz. Después vendría la parte analítica para cada variante, tal como el porcentaje de "compras" en la web, y podríamos decidir qué variante introduce una mejora real. En [este enlace](https://en.wikipedia.org/wiki/A/B\_testing) se da una explicación sobre el proceso bastante completa.

Vale, pues esto suena bien, pero ¿qué hay del rendimiento y seguridad que nos ofrece la CDN? ¿Podemos implementar un modelo _A/B testing_ sin renunciar a ella?

La CDN y su sistema de cacheo (Varnish) cachean las respuestas que devuelve el origen por completo, por lo que la mayoría de las peticiones de los clientes no llegan al servidor de origen. Si el sistema de _A/B testing_ depende de una lógica ubicada en origen, aunque un cliente asignado a la variante A nos solicite la web, éste podría recibir la variante opuesta cacheada en su lugar (y viceversa). Esto no es para nada lo que queremos y cualquier información obtenida en el experimento del _A/B testing_ sería nula.

### Solución

En realidad existen múltiples formas de solucionar este problema con las herramientas que tenemos a nuestro alcance, vamos a ver un ejemplo sencillo, donde separamos a los clientes por su dirección IP.

```javascript
sub vcl_recv {  
    if (req.url ~ "^/carrito"){   
        set req.http.abtesting = "A";  
        if (req.http.True-Client-IP ~ "[0-2]$") {  
            set req.http.abtesting = "B";  
        }
        set req.http.X-Vary-TCDN = req.http.X-Vary-TCDN + ",abtest:" + req.http.abtesting;
    }  
}
```

En el ejemplo anterior, vemos cómo se establece una cabecera:`abtesting` para distinguir entre las distintas variantes, después, dependiendo de la IP del cliente, le asignamos una distinta. Esto lo podríamos hacer en base a cualquier otra cabecera u otros parámetros de la request. Aquí estamos haciendo un balanceo entre las variantes del 30%/70%, ya que las IPs que terminen en 0, 1 o 2 irían con la variante "B", y el resto con "A".

Por último, establecemos la cabecera `X-Vary-TCDN` , una cabecera especial interna que gestiona las variantes de caché, __ incluyendo el valor de `abtesting` y guardando así una versión distinta del objeto en la caché para cada variante, logrando así obtener los beneficios tanto de la CDN como los del _A/B testing._

En cuanto al servidor de origen, éste sólo tendría que servir la variante adecuada según la cabecera `abtesting` cuando se produzca un _miss_ en el sistema de caché.

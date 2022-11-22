# Forzando el no-cache

## Lo que nos gusta a todos: enviar las cabeceras desde origen

En Transparent Edge somos partidarios de que nuestros clientes tengan la mayor autonomía. Por eso animamos siempre a que la configuración de la CDN sea lo más simple posible y que las cabeceras de caché, siempre que se pueda, se envíen desde los servidores de origen. En este [enlace](../../guias/configurar-mis-servidores-para-enviar-cabeceras-de-cache.md) tienes información sobre cómo hacerlo.

Para forzar que un objeto no se cachee, puedes configurar la cabecera _Cache-Control_ con cualquiera de estos valores:

```
no-cache
no-store
max-age=0
private
must-revalidate
```

Aunque cada una tiene pequeñas diferencias, en la práctica tienen el mismo efecto, que es que no se cachee el contenido. Si nos tenemos que decantar por una, usaremos _**No-Cache**_**.**

## Forzar la cabecera de _No-Cache_ desde Transparent Edge&#x20;

Aquí también vas a tener alternativas. Nosotros te vamos a proponer dos que posiblemente cubran el 95% de los casos de uso.

#### No cachear el objeto nunca en la CDN sin importar lo que venga de origen

En esta opción vamos a hacer que el objeto nunca se cachee en la CDN independientemente de las cabeceras de caché que vengan de origen. Para ello usaremos la cabecera _**req.http.TCDN-Command**_ dentro de la función _vcl\_recv_.

```javascript
sub vcl_recv {    
    if ((bereq.http.host == "www.transparentedge.eu") && (bereq.url ~ "/my-new-url")) {
        set req.http.TCDN-Command = "pass, " + req.http.TCDN-Command;
    }
} 
```

Este fragmento de código forzará que el objeto se salte la caché de Varnish y, por tanto, nunca se almacenará en la CDN. Esta configuración no afecta sobre las cachs del navegador. Si buscas que sí lo haga, tal vez sea mejor usar la siguiente opción.

#### No cachear el objeto en la CDN reescribiendo las cabeceras de origen

```javascript
sub vcl_backend_response {    
    if ((bereq.http.host == "www.transparentedge.eu") && (bereq.url ~ "/my-new-url")) {
            unset beresp.http.Cache-Control;
            set beresp.http.Cache-Control = "max-age=0";
            set beresp.ttl = 0s;
    }
} 
```

Con este código vamos a sobreescribir las cabeceras que vengan de origen y a ponerles el tiempo que nosotros queremos almacenar en caché ese objeto. En nuestro caso son 0s, tanto en el TTL interno, que forzará que ese objeto se guarde en la caché de Transparent Edge, como en la cabecera de caché que queremos que se propague al navegador (_Cache-Control_).

[Aquí](funcionalidades/reescritura-de-cabeceras.md) puedes consultar también cómo hacer reescrituras de cabeceras con más detalle.

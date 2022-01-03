# Web Application Gateway

Es común usar la CDN como un proxy intermedio para cachear el contenido, acelerar la entrega o proveer una capa extra de seguridad a nuestros sitios web, pero existen casos de uso menos comúnmente utilizados que son muy útiles en arquitecturas complejas. Hoy vamos a ver un ejemplo, y es el de usar una CDN como un _application gateway_, o _application-level proxy_.

Una CDN como Transparent Edge Services no deja de ser un concentrador de las peticiones a tu dominio, por lo que cualquier lógica de enrutado, filtrado, service mesh… puede realizarse en esta capa.

Tener disponible un lenguaje como _VCL_ (_Varnish Configuration Language_) para definir lógica a este nivel permite hacer todo tipo de virguerías y operaciones que harán las delicias de cualquier DevOps. Las posibilidades son amplias para cualquier app o sitio web que pase por Transparent CDN, que permite _SSL-offloading_ (protocol downgrade), end-to-end SSL, WAF, routing basado en URL, múltiples backend en función de criterios arbitrarios y personalizados, A/B testing, canary deployments, HTTP-header feature-flags y un largo etcetera.

Como las funcionalidades se ven con ejemplos, pongamos una API basada en microservicios, repartidos en varios hosts. Con un Application Gateway podemos hacer que todos los servicios estén disponibles vía [api.misitio.com](http://api.misitio.com). Los distintos microservicios podrían albergarse bajo distintas URL del mismo dominio [api.misitio.com](http://api.misitio.com): `/login`, `/estadisticas`, `/carrito`, cada uno en su propio _backend_, con un _VCL_ como el siguiente:

```
# Supongamos tres backends dados de alta:
# backend c0_login: login.miempresa.com:8080
# backend c0_stats: 11.22.33.44:80
# backend c0_shopcart: carrito.proveedordeterceros.com:443
sub vcl_recv {  
    if (req.url ~ "^/login"){  
        set req.backend_hint = c0_login.backend();  
    }else if (req.url ~ "^/estadisticas"){  
        set req.backend_hint = c0_stats.backend();  
    }else if (req.url ~ "^/carrito"){  
        set req.backend_hint = c0_shopcart.backend();  
    }  
}
#Cada oveja con su pareja: ap  
```

Este _VCL_ se puede complicar todo lo que se necesite, añadiendo validaciones, _throttling_, redirecciones, reescritura de URLs… Aquí abajo se muestran algunos ejemplos.

```
sub vcl_recv {  
    if (req.url ~ "^/login"){  
        set req.backend_hint = c0_login.backend();  
        # Solo permitimos acceder a estas URLs a nuestra IP de la oficina  
        if (! req.http.True-Client-Ip == "12.34.56.78"){  
            error 403 "The power of Christ compels you!";  
        }  
    }else if (req.url ~ "^/estadisticas"){  
        set req.backend_hint = c0_stats.backend();  
        # Hay mucho forofo de la estadística, vamos a intentar que no saturen de peticiones limitando a 10 req/s  
        set req.http.x-ratelimit = 30;  
    }else if (req.url ~ "^/carrito"){  
        set req.backend_hint = c0_shopcart.backend();
        # El carrito es de terceros, y tiene una URL distinta que quiero ocultar a los clientes:
        set req.url = "/third-parties/aef5677c321bb761c/"  
        # Aqui hacemos un poco de AB testing, seteando un header que el origen tendra en cuenta para devolver una version u otra:  
        set req.http.abtesting = 0;  
        if (req.http.True-Client-IP ~ "[0-2]$") { Si la ip del cliente acaba en 0,1 ó 2, cambiamos la cabecera, para que el backend devuelva una u otra version.  
            set req.http.abtesting = 1;  
        }  
    }  
} 
```

Las posibilidades son infinitas, así que si tienes algo en mente y quieres que te echemos una mano, contáctanos en soporte@transparentedge.eu&#x20;

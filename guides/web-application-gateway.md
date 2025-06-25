# Web Application Gateway

It is common to use a CDN as an intermediate proxy to cache content, accelerate delivery, or provide an extra layer of security to our websites. However, there are less commonly used use cases that are very useful in complex architectures. Today, we are going to see an example, which is using a CDN as an application gateway or application-level proxy.

A CDN like Transparent CDN acts as a hub for requests to your domain, so any routing, filtering, service mesh, etc., can be done at this layer.

Having a language like VCL (Varnish Configuration Language) available to define logic at this level allows for all kinds of tricks and operations that will delight any DevOps professional. The possibilities are extensive for any app or website that goes through Transparent CDN, which supports SSL-offloading (protocol downgrade), end-to-end SSL, WAF, URL-based routing, multiple backends based on arbitrary and custom criteria, A/B testing, canary deployments, HTTP-header feature flags, and much more.

Since functionalities are better understood with examples, let's consider an API based on microservices distributed across multiple hosts. With an Application Gateway, we can make all services available via [api.mysite.com](https://www.api.mysite.com). The different microservices can be hosted under different URLs of the same domain [api.mysite.com:](https://www.api.mysite.com) `/login, /stats, /cart,` each with its own backend. Here's an example VCL configuration:

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

This VCL can be as complex as needed, incorporating validations, throttling, redirects, URL rewriting, and more. Below are some examples:

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

The possibilities are endless, so if you have something in mind and you want us to help you, contact us at [soporte@transparentcdn.com](mailto:soporte@transparentcdn.com)&#x20;

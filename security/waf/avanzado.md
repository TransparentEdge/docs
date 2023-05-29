# Configuración

Esta implementación avanzada de WAF protegerá tus sitios _web_ de una manera más efectiva.

Para activar el WAF tan sólo tienes que activar nuestra cabecera `TCDN-WAF-Enabled`.

Por ejemplo, si quisiéramos activar el WAF en nuestro dominio `mi-dominio.es`, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
    }
}
```

## Desactivación condicional

Si deseas desactivar el WAF bajo ciertas condiciones, se podría hacer simplemente un `unset` a la cabecera previamente asignada.

Por ejemplo, si quisiéramos activar el WAF en nuestro dominio `mi-dominio.es`, pero excluyendo las URLs que _cuelgan_ de `/path/sin/waf/`, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/sin/waf/") {
            unset req.http.TCDN-WAF-Enabled;
        }
    }
}

```

Sin embargo, ésta dista de ser la mejor opción.

El WAF proporciona, en su lugar, la cabecera `TCDN-WAF-Set-SecRuleEngine` que nos permite ajustar el comportamiento del _motor_ (_engine_) de reglas. Esta cabecera acepta tres valores:

* `#On`. Se trata del comportamiento por defecto: el WAF lleva a cabo las acciones necesarias para bloquear aquellas peticiones (_requests_) consideradas _peligrosas_.
* `#Off`. Desactiva temporalmente el WAF.
* `#DetectionOnly`. En este caso, se llevan a cabo las acciones necesarias para identificar aquellas peticiones (_requests_) consideradas _peligrosas_ pero, como contrapartida, se permite su _paso_ a través del WAF. Este comportamiento es útil para realizar pruebas preliminares a fin de detectar posibles falsos positivos y, en consecuencia, poder incluir posteriormente las [excepciones](avanzado.md#inclusion-de-excepciones) que sean necesarias, si las hubiera.

Así, volviendo al ejemplo anterior, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/sin/waf/") {
            set req.http.TCDN-WAF-Set-SecRuleEngine = "#Off";
        }
    }
}
```

## Inclusión de excepciones

Si observas que el WAF está considerando como _peligrosas_ ciertas peticiones (_requests_) que, sin embargo, son perfectamente válidas; es decir, si el WAF está presentando falsos positivos, puedes incluir excepciones ante tales casuísticas mediante la cabecera `TCDN-WAF-Allow-Rule-Exceptions`.

Siguiendo con el ejemplo anterior, si observamos que las peticiones (_requests_) contra URLs que _cuelgan_ de `/path/completamente/seguro/` están siendo bloqueadas por el WAF al interpretar que se están violando las reglas `ruleID_1`, `ruleID_2`, `ruleID_3`, ..., `ruleID_n`, podemos indicar que, en este caso, estas correspondencias sean tratadas como excepciones. Para ello, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# WAF avanzado
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/sin/waf/") {
            set req.http.TCDN-WAF-Set-SecRuleEngine = "#Off";
        }
        if (req.url ~ "^/path/completamente/seguro/") {
            set req.http.TCDN-WAF-Allow-Rule-Exceptions = "ruleID_1 ruleID_2 ruleID_3 ... ruleID_n";
        }
    }
}
```

Obviamente, éstos no son más que pequeños ejemplos de casos de uso muy específicos. Si tienes cualquier duda respecto a cómo integrar esta funcionalidad en tu propio dominio, por favor, no dudes en contactarnos a través de la dirección de correo electrónico help+cdn@transparentedge.eu.


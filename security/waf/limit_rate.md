# Límite de tráfico

Esta función nos permite limitar el número de peticiones que se hacen a nuestro sitio _web_. Esta restricción puede estar basada en la dirección IP del cliente (`True-Client-IP`), una _cookie_, una URL o cualquier otra cosa que tenga sentido para tu aplicación.

Esta función se invoca a través de nuestra cabecera `TCDN-Command`; así, debemos incluir el valor `limit_rate` junto con el conjunto de parámetros, separados por el carácter _dos puntos_ (`:`), necesarios por ésta. Concretamente, la sintaxis de la función es la siguiente: `limit_rate:`_`<nombre>`_`:`_`<límite>`_`:`_`<periodo>`_`[:`_`<tiempo de bloqueo>`_`][:captcha]`. _`<nombre>`_ no es más que el identificador que empleamos para denominar a la regla que estamos configurando. _`<límite>`_ determina el número máximo de peticiones que se aceptarán dentro del _`<periodo>`_ indicado. Opcionalmente, los parámetros _`<tiempo de bloqueo>`_ y `captcha` nos permiten, respectivamente, establecer la duración durante la cual se denegarán peticiones una vez alcanzado el límite establecido en primer término y mostrar un [CAPTCHA](captcha.md) cuando este límite sea alcanzado.

Así, si el parámetro `captcha` se encuentra presente y se llega a alcanzar el límite fijado, se le mostrará al usuario un CAPTCHA que tendrá que validar para continuar con la navegación. Esta validación tendrá una validez de cinco minutos. De tal modo que si, en el transcurso de los cinco minutos sucesivos a dicha validación, este usuario vuelve a alcanzar el límite referido, se le aplicará, en su caso, el tiempo de bloqueo definido; y, si la validación ha expirado y el usuario vuelve a superar este umbral, un nuevo CAPTCHA le será mostrado.

Por ejemplo, si en nuestro dominio `mi-dominio.es`quisiéramos limitar cada usuario a un máximo de 20 peticiones (_requests_) por segundo , discriminando éste por su dirección IP y, una vez alcanzado dicho límite, bloquear dicho usuario durante 30 segundos y obligarle a validar un CAPTCHA, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# limit_rate
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-Command = "limit_rate:" + req.http.True-Client-IP + ":20:1s:30s:captcha";
    }
}
```

De este modo, una vez alcanzado el límite fijado, las sucesivas peticiones del usuario afectado darán como respuesta bien un _status code_ 429 (_Too Many Requests_) o bien un 418 (_Robots are not allowed here!_), si la validación del CAPTCHA hubiese sido incorrecta.

Obviamente, estos límites se pueden establecer para todo el _site_ o para una parte de él y discriminar en función de distintos criterios, no sólo la dirección IP de usuario; por ejemplo, podríamos tener en consideración una cuota de uso de 30 peticiones por minuto por API _key_:

```c
# limit_rate
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-Command = "limit_rate:API key=" + req.http.API-Key + ":30:60s";
    }
}
```

O, quizá, permitir tan sólo unas pocas peticiones POST o PUT por usuario:

```c
# limit_rate
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        if (req.method == "POST" || req.method == "PUT") {
            set req.http.TCDN-Command = "limit_rate:" + req.http.True-Client-IP + ":2:10s";
        }
    }
}
```

Obviamente, éstos no son más que pequeños ejemplos de casos de uso muy específicos. Si tienes cualquier duda respecto a cómo integrar esta funcionalidad en tu propio dominio, por favor, no dudes en contactarnos a través de la dirección de correo electrónico [soporte@transparentcdn.com](mailto:soporte@transparetncdn.com).

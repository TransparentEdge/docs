# CAPTCHA

Esta función nos permite bloquear las peticiones que se hacen a nuestro sitio _web_ por parte de _bots_ o, dicho con otras palabras, garantizar que el tráfico recibido proviene de seres humanos.

Con este propósito en mente, los CAPTCHAs (_Completely Automated Public Turing test to tell Computers and Humans Apart_) consisten en pequeñas pruebas (desafíos) automatizadas, es decir, realizadas por una máquina y, en consecuencia, sin intervención humana, y que permiten discriminar cuándo el usuario es un ser humano y cuándo un programa automático (_bot_). Concretamente, la implementación de esta función se ha realizado basándose en la solución [reCAPTCHA](https://www.google.com/recaptcha/about/) ofrecida por Google.

Esta función se invoca a través de nuestra cabecera `TCDN-Command`; así, debemos incluir el valor `show-captcha`. Concretamente, la sintaxis de la función es la siguiente: `show-captcha[:`_`<ttl>`_`]`.

Internamente, cuando un usuario resuelve de manera satisfactoria el desafío presentado por el CAPTCHA, se asigna una _cookie_ en su navegador llamada `TCDN-Captcha-UID`. Esta _cookie_ permite identificar de manera inequívoca la sesión del usuario y, en consecuencia, evita que se le vuelva a requerir una y otra vez que supere el desafío del CAPTCHA. Sin embargo, dicha _cookie_ tiene un tiempo de vida (`ttl`, _time to live_) finito: una hora; transcurrida ésta, un nuevo desafío deberá ser resuelto por el usuario. No obstante, el parámetro opcional _`<ttl>`_ permite, precisamente, controlar el tiempo de vida de esta _cookie_ de sesión.

Por ejemplo, si quisiéramos limitar la presencia de _bots_ en nuestro dominio `mi-dominio.es`, pero sin importunar excesivamente a nuestros usuarios legítimos, podríamos plantear la presencia de un CAPTCHA con un TTL de ocho horas. Así, nos bastaría con desplegar desde el [panel](../../getting-started/dashboard/) una [configuración](../../getting-started/dashboard/autoprovisionamiento/) [VCL](../../config/vcl/) similar a la siguiente:

```c
# show-captcha
sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-Command = "show-captcha:28800s";
    }
}
```

De este modo, si el usuario resuelve satisfactoriamente el desafío presentado por el CAPTCHA, se le asignará la _cookie_ `TCDN-Captcha-UID` y podrá continuar navegando con normalidad durante las siguientes ocho horas, cuando se le volverá a presentar el CAPTCHA. Alternativamente, si el usuario es incapaz de resolver la prueba, obtendrá como respuesta un _status code_ 403 (_Robots are not allowed here!_).

Obviamente, éste no es más que un pequeño ejemplo de un caso de uso muy específico. Si tienes cualquier duda respecto a cómo integrar esta funcionalidad en tu propio dominio, por favor, no dudes en contactarnos a través de la dirección de correo electrónico [soporte@transparentcdn.com](mailto:soporte@transparetncdn.com).


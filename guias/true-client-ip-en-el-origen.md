# True-client-ip en el origen

### &#x20;¿Cómo puedo logar  la cabecera True-Client-IP en los logs de Apache?

Es un paso relativamente sencillo, y se realiza incluyendo la cabecera True-Client-IP dentro de la instrucción "LogFormat" de apache dentro del fichero de configuración (normalmente httpd.conf)

Por ejemplo, supongamos que usted tiene activado los logs con formato "combined", y el formato original que usted tiene es algo así:

```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
```

Para que la dirección True-Client-IP aparezca en sus log, una vez activada la cabecera en la configuración de Transparent CDN, podría cambiarse a algo así:

```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%{True-Client-IP}i\" %q" combine
```

### ¿Como veo la dirección ip real del cliente desde mi aplicación PHP?

Al tratarse de una cabecera HTTP en la variable $\_SERVER de PHP se encuentran todas las cabeceras de request, por eso mediante $\_SERVER\['True-Client-IP'] puedes conocer la ip real del usuario que se conecta.

Alternativamente a esta solución, si no puedes o se antoja complicado cambiar el código PHP se puede hacer uso de la directiva **auto\_prepend\_file** en el php.ini. Esta directiva nos permite ejecutar un script en php antes de cualquier ejecución, por lo que debe ser un script ligero. Para el caso que nos afecta puedes hacer algo asi:

```
  ...
  auto_preprend_file = /usr/local/bin/set_ip.php
  ...
```

```
  if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
      $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_X_FORWARDED_FOR'];
  }
```

De esta manera si la aplicación hace uso de la cabecera $\_SERVER\['REMOTE\_ADDR'] no habria que tocar nada del código puesto que ya estamos igualmando la ip de cliente como lo que llega en la cabecera x-forwarded-for.

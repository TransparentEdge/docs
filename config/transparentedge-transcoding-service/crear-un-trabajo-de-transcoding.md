# Crear un trabajo de transcoding

El servicio de transcoding permite ajustar la calidad de los videos y convertirlos a los formatos y señales deseados, alojándolos donde se requiera. Para crear un trabajo de transcoding tiene que utilizar nuestra [API](https://api.transparentcdn.com/docs) y realizar la siguiente llamada al endpoint:

```
https://api.transparentcdn.com/v1/media/{company_id}/transcode
```

En esta petición de tipo **POST** cabe resaltar el tipo de cuerpo de petición, que deberá ser o bien _form-data_ o bien _x-www-form-urlencoded_. Este cuerpo de petición llevará un campo cuya clave debe ser **order** y en el campo valor un _JSON_ similar a este.

```
{
  "priority": 5,
  "origin": "ftp://usuario:password@ftp.com/published/video.mp4",
  "notifications": [
    {
      "email": "micorreo@misitio.com"
    }
  ],
  "destinations": [
    "ftp://usuario:password@ftp.tudireccionftp.com"
  ],
  "jobs": [
    {
      "label": "iphone",
      "filename": "watermark.mp4",
      "profile_id": 666
    }
  ],
  "thumbnails": [
    {
      "filename": "nombre.jpg",
      "label": "label",
      "width": "720",
      "height": "480",
      "time": 0.3
    }
  ]
}
```

Para realizar la petición, debemos haber habernos autenticado previamente. Si tienes dudas de cómo hacerlo puedes obtener información [aquí](https://docs.transparentedge.eu/api/autenticacion). Una vez que obtengamos el token, podríamos encolar el trabajo realizando una petición **POST** mediante **curl**. Con el objetivo de no repetirnos, tomaremos como ejemplo el anterior _JSON_, que habremos almacenado en una variable llamada _JDATA_ de esta forma:

```
JDATA='order={
        "priority":5,
        "origin":"ftp://usuario:contraseña@ftp.com/videos/ejemplo.mp4",
        etc..
        }'
```

Habiendo almacenado los datos de la petición en la variable _JDATA_ y obtenido el token de autorización, que aquí se encuentra ejemplificado como _YOUR\_API\_TOKEN_,  podríamos realizar la petición como se muestra a continuación.

```
curl -ksvH "Authorization: Bearer YOUR_API_TOKEN" -XPOST -H 'Content-type: application/x-www-form-urlencoded' -d "$JDATA" "https://api.transparentcdn.com/v1/media/{company_id}/transcode/"
```

En el cuerpo de la petición encontramos varios campos: el **origin** del archivo, es decir, de dónde será descargado, a dónde debemos notificar sobre el éxito o fracaso del trabajo, indicado en **notifications**. Por otro lado, **destinations** nos indica dónde debemos enviar el nuevo vídeo o vídeos formateados. El conjunto **jobs** refiere los diferentes trabajos que el servicio de transcoding debe realizar, con los nombres de los archivos resultantes y qué _transcoding profile_ deben utilizar. Así, resultarían de este ejemplo un único archivo de nombre "watermark.mp4" que utilizaría el _profile_ 15. Cabe reseñar también la posibilidad de incluir el argumento "priority" en nuestra petición. A través de los perfiles de transcodificación que tengamos asociados podremos incluir la opción de restringir el _bit rate_ de nuestros vídeos. Se trata de una opción que evaluará cuál es el _bit rate_ de vídeo menos pesado y tomará ese como referencia para hacer la transcodificación. Por ejemplo: si tenemos un vídeo con un _bit rate_ de 985 y nuestro perfil elegido está configurado para transcodificar videos a una tasa de 1200 el sistema elegirá la tasa menor para efectuar el trabajo. Actualmente no está la opción disponible desde el _dashboard_ por lo que si queremos elegir esta opción habrá que notificarselo al responsable técnico. No obstante, pronto podremos elegir nosotros esta opción en el apartado _"editar"_  de nuestros perfiles.

Si tenemos contratado el servicio de _Priority Transcoding_ podremos indicar qué prioridad queremos darle a este trabajo de transcodificación. Es un argumento que **no es obligatorio**, y que en el caso de tener el servicio mencionado anteriormente, hará que dicho trabajo se procese el primero de todos los que ya se hubieran encolado. El valor que podremos indicarle es un número entero entre el 5 o el 10. Respecto a los _transcoding profiles_ podemos elegir cualquiera de los perfiles que deseemos y que tengamos configurados. Estos pueden consultarse en el siguiente endpoint.

```
https://api.transparentcdn.com/v1/media/{company_id}/transcoding_profiles/
```

Esta dirección, como vemos, nos devolverá todos los perfiles de transcodificación que tengamos creados y disponibles para su uso. Una vez que tengamos el **id** de nuestro _transcoding profile_ seleccionado simplemente debemos sustituirlo por el "666" del campo "transcoding\_profile" que se encuentra en el ejemplo para encolar un trabajo en el servicio.

# Obtener información de los trabajos

Nuestra API posee diferentes endpoints que pueden ser utilizados para visualizar el estado de las diferentes órdenes de trabajo. Podremos obtener una lista de todas las órdenes de trabajo registradas para cada compañía lanzando una petición **GET**, previa [autenticación](https://docs.transparentedge.eu/api/autenticacion), a este endpoint: `https://api.transparentcdn.com/v1/media/{company_id}/transcode`

Obtendremos, como respuesta, una relación de todas las órdenes de trabajo que hayamos encolado para nuestro id de compañía, estén estas procesadas por el _TransparentEdge Transcoding Service_ o no. El formato será el de un array de objetos en el que apareceran los campos "_order\_id",_  con el identificador único del trabajo, _"status" ,_ que nos indica el estado actual de la petición __ y _"timestamp"_ el cual indicará el _timestamp_ en el que la petición de trabajo fue procesada por _TransparentEdge Transcoding Service._ Podríamos esperar una respuesta similar a esta:

```javascript
[
    {
        "id": "1631092552-1237",
        "created_on": "2021-09-08 09:15:55.875827+00:00",
        "status": "FINISHED"
    },
    {
        "id": "1631089188-8755",
        "created_on": "2021-09-08 08:19:50.679393+00:00",
        "status": "FINISHED"
    },
    {
        "id": "1630920819-1014",
        "created_on": "2021-09-06 09:33:40.051379+00:00",
        "status": "ERROR"
    },
    {
        "id": "1630920029-2222",
        "created_on": "2021-09-06 09:20:30.296758+00:00",
        "status": "FINISHED"
    },
    {
        "id": "1630919890-5510",
        "created_on": "2021-09-06 09:18:11.329456+00:00",
        "status": "FINISHED"
    },
    {
        "id": "1630919313-6156",
        "created_on": "2021-09-06 09:08:34.152847+00:00",
        "status": "FINISHED"
    },
    {
        "id": "1630919028-9762",
        "created_on": "2021-09-06 09:03:49.318187+00:00",
        "status": "FINISHED"
    },
]
```

Para obtener información de un trabajo en particular, bastaría con hacer una llamada a `https://api.transparentcdn.com/v1/media/{company`_`id}`_`/transcode/{order_id}` donde podríamos obtener información más detallada del estado de la orden, así como de los sub trabajos que la componen. La respuesta que obtendremos será similar a esta:

```javascript
{
   "order_id":"163104952-137",
   "transcodingupload_set":[
      {
         "upload_url":"ftp://ftp.tuservidor.com",
         "upload_protocol":"FTP",
         "upload_user":"usuario"
      }
   ],
   "transcodingdownload":{
      "download_url":"ftp://ftp.tuservidor.com/otro.mp4",
      "download_protocol":"FTP",
      "download_user":"usuario"
   },
   "transcodingnotification_set":[
      {
         "callback_url":null,
         "email":"correo@tuservidor.com"
      }
   ],
   "transcodingjob_set":[
      {
         "profile":{
            "id":92,
            "name":"Prueba escalado"
         },
         "filename":"nombre.mp4",
         "label":"prioridad alta",
         "progress":0.0
      },
      {
         "profile":{
            "id":86,
            "name":"Test HLS"
         },
         "filename":"nombre2.mp4",
         "label":"prioridad media",
         "progress":0.0
      }
   ],
   "duration":95.143,
   "size":12118497,
   "filename":"nombre.mp4",
   "label":"prioridad alta",
   "status":"FINISHED",
   "timestamp":"2021-09-08 09:15:55.875827+00:00",
   "comment":"Order 163104952-11567 finished"
}
```


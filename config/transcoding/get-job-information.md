# Get job information

Our API has different endpoints that can be used to view the status of the different work orders. We can obtain a list of all the work orders registered for each company by launching a **GET** request, after [authentication](https://docs.transparentedge.eu/api/autenticacion), to this endpoint:

`https://api.transparentcdn.com/v1/media/{company_id}/transcode`

We will receive a list of all the work orders that we have enqueued for our company ID, whether they are processed by the TransparentEdge Transcoding Service or not. The format will be an array of objects containing the fields "_order\_id_," which holds the unique identifier of the job, "_status_," indicating the current state of the request, and "_timestamp_," indicating the timestamp when the work request was processed by the TransparentEdge Transcoding Service. We can expect a response similar to this:

```
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

To obtain information about a specific job, you can make a request to the following endpoint: `https://api.transparentcdn.com/v1/media/{companyid}/transcode/{order_id}`. This API call will provide you with more detailed information about the status of the order, as well as the sub-jobs that make it up. The response you will receive will be similar to this:

```
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

# Create a transcode job

The transcode service allows you to adjust the quality of the videos and convert them to the desired formats and signals, lodging them where required. To create a transcode job, you have to use our [API](https://api.transparentcdn.com/docs) and make the following call to the endpoint:

```
https://api.transparentcdn.com/v1/media/{company_id}/transcode
```

In this **POST** type request, it is important to stand out the type of request body, which must be either _form-data_ or _x-www-form-urlencoded._ This request body will have a field whose key must be **order** and in the value field a JSON similar to this one.

```
{
  "priority": 5,
  "origin": "ftp://usuario:password@ftp.com/published/video.mp4",
  "destinations": [
    "ftp://usuario:password@ftp.tudireccionftp.com",
    "s3://bucket-name"
  ],
  "jobs": [
    {
      "label": "iphone",
      "filename": "watermark.mp4",
      "profile_id": 666,
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
  ],
  "notifications": [
    {
      "email": "micorreo@misitio.com",
      "call": "https://api.cliente.com/notify/transcoding"
    }
  ]
}
```

To make the request, we have previously authenticated ourselves. If you have questions about how to do it, you can get information [here.](https://docs.transparentedge.eu/api/autenticacion) Once we get the token, we could queue the job by making a **POST** request using **curl.** In order not to repeat ourselves, we will take the previous JSON as an example, which we will have stored in a variable called JDATA in this way:

```
JDATA='order={
        "priority":5,
        "origin":"ftp://username:password@ftp.com/videos/example.mp4",
        etc..
        }'
```

Having stored the request data in the JDATA variable and obtained the authorization token, which is exemplified here as YOUR\_API\_TOKEN, we could make the request as shown below.

```
curl -ksvH "Authorization: Bearer YOUR_API_TOKEN" -XPOST -H 'Content-type: application/x-www-form-urlencoded' -d "$JDATA" "https://api.transparentcdn.com/v1/media/{company_id}/transcode/"
```

In the request body we find several fields: the **origin** of the file, meaning, where it will be downloaded from, where we should notify about the success or failure of the job, indicated in **notifications.** On the other hand, **destinations** tells us where we should send the new formatted video or videos. The **jobs** set refers to the different jobs that the transcoding service should perform, with the names of the resulting files and which transcoding profile to use. In this way this example would result in a single file named “watermark.mp4” that would use profile 15. It is also worth noting the possibility of including the “priority” argument in our request. Through the transcoding profiles that we have associated, we can include the option to restrict the bit rate of our videos. This is an option that will evaluate the least heavy video bit rate and take that as a reference to do the transcoding. For example: if we have a video with a bit rate of 985 and our chosen profile is configured to transcode videos at a bit rate of 1200, the system will choose the lower rate to perform the job. Currently, the option is not available from the dashboard, so if we want to select this option, we will have to notify the technical manager. However, soon we will be able to pick this option in the “edit” section of our profiles.

If we have contracted the Priority Transcoding service, we can indicate what priority we want to give to this transcoding job. It is an argument that is **not mandatory**, and that in the case of having the service mentioned above, it will cause said job to be processed first of all those that have already been queued. The value that we can indicate is an integer between 5 or 10. Regarding the transcoding profiles, we can choose any of the profiles that we want and that we have configured. These can be consulted in the following endpoint.

```
https://api.transparentcdn.com/v1/media/{company_id}/transcoding_profiles/
```

This address will return all the transcoding profiles that we have created and available for use. Once we have the **ID** of our selected transcoding profile, we simply have to replace it with “666” from the “transcoding\_profile” field found in the example to enqueue a job in the service.

There is an option to apply **DRM** to your video as part of the post-processing options. This feature is currently available only via an API POST request.

To use it, you need to add another field named `"drm"` under the `"post-process"` key in your request. The `"drm"` object includes the following fields:

* **`key`**: The encryption key in hexadecimal format (32 characters – _mandatory_).
* &#x20;**`key_id`**: The ID of the encryption key (32 characters – _mandatory_).
* **`key_iv`**: The IV (Initialization Vector) of the encryption key (32 characters – _optional_).
* **`pssh`**: The PSSH data for the DRM  (_optional_).
* **`encryption_scheme`**: The encryption scheme, which can be `"cenc"`, `"cbc1"`, `"cens"`, or `"cbcs"` (_default: "cbcs"_ - _optional_).

```
{
  "priority": 5,
  "origin": "ftp://usuario:password@ftp.com/published/video.mp4",
  "destinations": [
    "ftp://usuario:password@ftp.tudireccionftp.com",
    "s3://bucket-name"
  ],
  "jobs": [
    {
      "label": "iphone",
      "filename": "watermark.mp4",
      "profile_id": 666,
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
  ],
  "notifications": [
    {
      "email": "micorreo@misitio.com",
      "call": "https://api.cliente.com/notify/transcoding"
    }
  ],
  "post_process": [
    {
      "drm": {
        "key": "abcdefabcdefabcdefabcdefabcdef12",
        "key_id": "1234567890abcdef1234567890abcdef",
        "pssh": "psshdata2",
        "encryption_scheme": "cbcs"
      }
    }
  ]
}
```

It is also possible to create a new transcoding job through our [dashboard.](https://dashboard.transparentcdn.com/) To do this, you need to expand the options of the button located at the top of the transcoding job list and follow the steps indicated in the displayed modal.

<figure><img src="../../.gitbook/assets/transcoding-new-job.png" alt=""><figcaption></figcaption></figure>

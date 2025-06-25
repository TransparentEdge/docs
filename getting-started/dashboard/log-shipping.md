# Log shipping

The log sending feature is included in any service package. It just needs to be activated and set up for it to start.

### Format and frequency

In batch mode, logs are extracted, compressed and sent every hour. On average, there is one compressed log file for each edge node belonging to our delivery platform, provided requests have been processed on these and related with any of the sites involved.

Every compressed log file has the following name mask::

```
<YYYYMMDDHH>-<CID>-<COUNTRY CODE>-<HASH>.log.gz
```

Thereby:

1. The first field corresponds to the send date (`YYYYMMDDHH`).
2. The second is the unique client /company ID.
3. The next one represents the country code associated to the geographical location of the edge node meeting those requests.
4. Last but not least, a hash code is included which identifies the edge node itself. This field is used internally for traceability and debugging purposes.

For instance:  `2023022010-4-spain-c7658c0dd514f6523f7a98ddd0dbb448.log.gz`

### Log shipping to a FTP / sFTP server

This choice allows the log to be sent to an arbitrary FTP / sFTP server.

To set it up properly, simply specify the correct protocol and fill in the gaps with the remaining parameters required. Once this choice is activated and its parameters validated, sending will begin within the next hour.

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### Log shipping to an Amazon S3 bucket compatible

This choice allows the log sending to a compatible Amazon S3 bucket.

In order to properly set it up, an access and secret key credential pair must be provided that must also be associated with a policy allowing, at least, file uploading into the bucket that is to be used.

`Bucket` field allows two name masks:

* `<ENDPOINT URL>/<BUCKET NAME>`
* `<BUCKET NAME>` (just in case of Amazon S3 bucket)

`<ENDPOINT URL>` corresponds to the URL hosting the S3 service. In the scenario where the bucket is hosted in Amazon S3, it is possible to use just the `<BUCKET NAME>` as the sole parameter.

Anywise, if it must also be included, for instance, to point to a specific geographical region, the bucket name must be removed from the domain itself: `https://tcdn-testing.s3.us-east-1.amazonaws.com/tcdn-testing would become https://s3.us-east-1.amazonaws.com/tcdn-testing`. If this is not done, the bucket name will be included at the beginning of the remote path.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### Sorting out compressed log files by date and time

When log sending is being configured, `path` field, which represents the remote path where compressed log files will be stored, allows for a set of special date--related masks:

* `%Y`, year
* `%m`, month
* `%d`, day
* `%H`, hour

For instance, if the value of `path` field is `/tcdn-logs/%Y/%m/%d` and today is February 20, 2023, when the process is executed, the compressed log files will be remotely stored in the path `/tcdn-logs/2023/02/20`.

### Log sending by streaming

Furthermore, there is another way of log sending to the client, in this case, in real time. Please check [this link](../../guides/streaming-logs.md) out to see more details on how to configure this feature.

{% hint style="info" %}
Remember that every single task you can do from our [dashboard](https://dashboard.transparentcdn.com/) can also be accomplished from our [API](../faq/glosario/api.md).
{% endhint %}

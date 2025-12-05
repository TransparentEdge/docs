# Log shipping

The log shipping feature is included in all service plans. It only needs
to be activated and configured to start working.

### Format and frequency

In batch mode, logs are collected, compressed, and sent every hour.
Typically, one compressed log file is generated for each edge node in
our delivery platform, as long as that node has processed requests for
any associated site.

Each compressed log file follows this naming pattern:

```
<YYYYMMDDHH>-<CID>-<COUNTRY CODE>-<HASH>.log.gz
```

Where:

1.  `YYYYMMDDHH` is the timestamp of the batch.
2.  `<CID>` is the unique client/company ID.
3.  `<COUNTRY CODE>` is the country associated with the edge node that served the requests.
4.  `<HASH>` is an internal hash that identifies the edge node, used for traceability and debugging.

Example:

```
2023022010-4-spain-c7658c0dd514f6523f7a98ddd0dbb448.log.gz
```

### Log shipping to an FTP / SFTP server

This option sends logs to any FTP or SFTP server.

To configure it, select the appropriate protocol and fill in the
required connection parameters. After activation and validation, log
delivery will begin within the next hour.

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### Log shipping to an Amazon S3--compatible bucket

This option sends logs to an Amazon S3 bucket or any S3-compatible service.

To configure it, provide an access key and secret key with a policy that
allows at least file uploads to the target bucket.

The `Bucket` field accepts two formats:

-   `<ENDPOINT URL>/<BUCKET NAME>`
-   `<BUCKET NAME>` (for Amazon S3 buckets)

`<ENDPOINT URL>` is the base URL of the S3 service. For Amazon S3, you may use only the bucket name.
If including the endpoint, for example, to target a specific
region, make sure the bucket name is *not* duplicated in the domain.

For example:

`https://tcdn-testing.s3.us-east-1.amazonaws.com/tcdn-testing` should be written as: `https://s3.us-east-1.amazonaws.com/tcdn-testing`.
Otherwise, the bucket name will be added automatically at the start of the remote path.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### Sorting compressed log files by date and time

The `path` field defines the remote folder where log files will be
stored and accepts the following date-related placeholders:

-   `%Y`: year
-   `%m`: month
-   `%d`: day
-   `%H`: hour

For example, if the `path` is `/tcdn-logs/%Y/%m/%d` and today is
February 20, 2023, the logs will be stored in `/tcdn-logs/2023/02/20`.

### Log streaming

Logs can also be delivered in real time. See [Streaming Logs](../../guides/streaming-logs.md) for details on configuring log
streaming.

{% hint style="info" %}
Remember: anything you can do from the [dashboard](https://dashboard.transparentcdn.com/) can also be done through our [API](../faq/glosario/api.md).
{% endhint %}

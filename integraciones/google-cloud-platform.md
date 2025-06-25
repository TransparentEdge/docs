# Google Cloud Platform

A very evident use case is to place Transparent CDN in front of your Google Cloud Platform (GCP) infrastructure, even if you have the **Cloud CDN** service activated in GCP. Not only will you benefit from a wider range of CDN features, but you will also significantly reduce the cost of GCP. This cost reduction is mainly due to the fact that Transparent CDN offers lower rates for traffic compared to GCP, resulting in potential cost savings of 35-45% depending on the specific case.

All of this is possible without making any changes to your AWS platform. All you need to do is configure the CNAME provided by AWS as the [origin](../getting-started/dashboard/auto-provisioning/sites.md) for Transparent CDN.

### Google Cloud Storage

Similarly, just like you can configure Cloud CDN or your GCP auto-scaling platform by setting the CNAME of the service as the origin for Transparent Edge, you can do the same with **Cloud Storage.** By caching the content in Transparent Edge and serving all the objects from your Cloud Storage bucket through our edge locations, you can significantly reduce the traffic reaching Cloud Storage.

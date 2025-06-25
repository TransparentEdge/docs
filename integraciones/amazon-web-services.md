# Amazon Web Services

A very clear use case is to place Transparent Edge Services in front of your **Amazon Web Services** (AWS) platform, even if you have the **CloudFront** service. Not only will you have a wider range of CDN functionalities, but you will also significantly reduce the cost of **AWS**. This reduction is mainly due to the lower pricing for traffic in Transparent Edge compared to AWS. In some cases, this cost savings can be as high as 35-45%, depending on the specific scenario.

All of this is possible without making any changes to your AWS platform. All you have to do is configure the CNAME provided by AWS as the [origin](../getting-started/dashboard/auto-provisioning/sites.md) of Transparent Edge.

### S3

Similarly, just like you can place CloudFront or your auto-scaling platform by configuring the CNAME of the AWS service as the origin of Transparent Edge, you can do the same with **S3**. This allows you to significantly reduce the traffic reaching S3 by caching the content in Transparent Edge and serving all the objects in that bucket from our edge servers.

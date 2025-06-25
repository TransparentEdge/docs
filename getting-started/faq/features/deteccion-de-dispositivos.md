# Device detection

Transparent Edge can use the User-Agent header to determine the type of device that is connecting and categorize it as a desktop, mobile, or tablet. To identify the device, Transparent Edge includes a header called X-Device in both the request to the origin and the response to the client. This header can have three values:

* desktop
* mobile
* tablet

Depending on the value of this header, Transparent Edge saves a version of each object for that device, making it possible to serve different content for each type of device under the same URL. If you want to find out how to do it, we explain it [here](https://docs.transparentedge.eu/guias/cachear-una-version-por-dispositivo).

### Force device

Even if the device is detected, it's possible to, for instance, force a mobile device to display a desktop version. We do this using the Force-Device cookie. If this cookie is present, it takes priority over X-Device.

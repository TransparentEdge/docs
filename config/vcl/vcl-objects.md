# VCL Objects

In VCL, there are different types of objects that you need to know. These objects can be accessed and modified from the VCL configuration.

* **req.** It is the request object. It is mainly accessed from the vcl\_recv function. When Varnish receives the request, this object is created and accessible to be accessed.
* **bereq.** It is the object sent to the backend. It is created just before sending the object to the backend or origin. It is based on the req object.
* **beresp.** It is the backend response. It contains the headers of the backend's response to Transparent CDN. If you want to modify them, this object is accessible from the **vcl\_backend\_response** function.
* **resp.** It is the HTTP response just before being sent to the client. You can modify this object in the **vcl\_deliver** function.
* **obj.** It is a read-only object and represents the object stored in the cache.


# A/B Testing

### What is A/B Testing and why would we want to implement it?&#x20;

Through A/B testing, we can expose users to two slightly different experiences, such as changing the color of a button in the interface. Then comes the analytical part for each variant, such as the percentage of "purchases" on the website, and we can decide which variant introduces a real improvement. [This link](https://en.wikipedia.org/wiki/A/B_testing) provides a comprehensive explanation of the process.&#x20;

Okay, this sounds good, but what about the performance and security offered by the CDN? Can we implement an A/B testing model without sacrificing it?&#x20;

The CDN and its caching system (Varnish) cache the responses returned by the origin server completely, so most client requests do not reach the origin server. If the A/B testing system relies on logic located at the origin server, even if a client assigned to variant A requests the website, they could receive the cached opposite variant instead (and vice versa). This is not what we want, and any information obtained in the A/B testing experiment would be invalid.

### Solution&#x20;

In reality, there are multiple ways to solve this problem using the tools at our disposal. Let's see a simple example where we separate clients based on their IP address.

```c
sub vcl_recv {  
    if (req.url ~ "^/carrito"){   
        set req.http.abtesting = "A";  
        if (req.http.True-Client-IP ~ "[0-2]$") {  
            set req.http.abtesting = "B";  
        }
        set req.http.X-Vary-TCDN = req.http.X-Vary-TCDN + ",abtest:" + req.http.abtesting;
    }  
}
```

In the previous example, we see how an `abtesting` header is set to distinguish between different variants. Then, based on the client's IP, we assign them a different variant. This could be done based on any other header or request parameter. In this case, we are balancing the variants with a split of 30% for "B" and 70% for "A", as the IPs ending in 0, 1, or 2 will be assigned to variant "B," and the rest to variant "A."

Lastly, we set the `X-Vary-TCDN` header, which is a special internal header that manages cache variants. It includes the value of `abtesting` and stores a different version of the object in the cache for each variant. This allows us to benefit from both the CDN and A/B testing.

As for the origin server, it only needs to serve the appropriate variant based on the `abtesting` header when a cache miss occurs.

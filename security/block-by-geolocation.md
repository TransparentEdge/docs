# Blocking Requests Geographically

By default, in Transparent Edge Services, we geolocate all requests that pass through our systems and send a header with the country code from which the request was made to the origin. This header is called geo\_country\_code.

```
geo_country_code: ES
```

For this functionality, we integrate with [GeoIP2 Enterprise Database](https://www.maxmind.com/en/solutions/geoip2-enterprise-product-suite/enterprise-database). This database has a **99.8%** accuracy for country-level geolocation.

It's also possible to send other data provided by this database as an HTTP header to the origin, such as the city. However, please note that the accuracy at the city level in this database is around 75% for Spain. You can check the accuracy by country [here.](https://www.maxmind.com/en/geoip2-city-accuracy-comparison)

The value of the geo\_country\_code header is the country code based on the standard [ISO 3166.](https://www.iso.org/obp/ui/#search)

If the system is unable to locate the user's IP, the header will contain the string 'Unknown'.

Using this header, we can make various decisions, such as redirection, serving specific content for that country, or simply geoblocking content.

For geoblocking, you can go to your [dashboard](https://dashboard.transparentcdn.com), navigate to **Provisioning, VCL Config**, and in the advanced section, duplicate the current production configuration. Then, insert the geoblocking code within the vcl\_recv function:

```javascript
sub vcl_recv{
    if (req.http.geo_country_code ~ "RU") { 
        call deny_request;
    } 
}

```

Likewise, we could redirect the website to a specific URL or site based on the user's origin. For example:

```javascript
sub vcl_recv{
    if (req.http.geo_country_code ~ "ES|AD|PT") { 
        set req.http.X-retSynth = "751,https://www.transparentcdn.com/es";
    } else {
        set req.http.X-retSynth = "751,https://www.transparentcdn.com/en"
    }
}
```

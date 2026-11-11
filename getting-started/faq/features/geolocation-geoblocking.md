# Geolocation and geoblocking

At Transparent Edge, by default, we geolocate all the requests that pass through our systems by sending a header to the origin with the code for the country where the request was made. This header is Geo\_Country\_Code.

```
geo_country_code: ES
```

We use the [GeoIP2 Enterprise Database](https://www.maxmind.com/en/solutions/geoip2-enterprise-product-suite/enterprise-database) for this functionality, which can detect the country with an accuracy of **99.8%**.

We can send other data provided by this database to the origin in the form of an HTTP header—the city, for instance—but bear in mind that the database can only detect cities with an accuracy of around 75% for Spain. You can check the accuracy data by country[ here.](https://www.maxmind.com/en/geoip2-city-accuracy-comparison)​

The value of the Geo\_Country\_Code header is the country code from the[ ISO 3166](https://www.iso.org/obp/ui/#search) standard.​

If the system is not able to locate the user’s IP address, it sends the Unknown string within the header.

This functionality gives us a range of possibilities for taking into account the end user’s location, such as:

* Blocking/allowing content from certain countries.
* Sending requests to a particular origin based on the end user’s geographic location.
* Serving special content by country.
* Rewriting headers based on the country requests come from.
* Establishing personalized rules based on the value of the header.

To see how to implement geoblocks with this header, click[ here.](https://docs.transparentedge.eu/security/bloqueando-peticiones-geograficamente)​

# geo\_country\_code

By default, Transparent Edge geolocates all requests that pass through our systems by sending a header to the origin with the country code from which the request was made. This is the **Geo\_Country\_Code** header.

```
geo_country_code: ES
```

If the system was unable to locate the user's IP, it sends the Unknown string in the header.

Thanks to this feature, there are a great many things we can do based on the location of the end user, such as:

* Block/permit content from certain countries.
* Send requests to different origins based on the geographical location of the end user.
* Serve special content by country.
* Rewrite headers based on the country the request came from.
* Custom rules based on that header.

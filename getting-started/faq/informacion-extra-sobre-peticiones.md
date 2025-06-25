# Predefined headers

All requests that pass through Transparent Edge automatically get a set of headers added to them that we think can be useful for the origin. They are as follows:

## True-Client-IP

Because CDNs function as reverse proxies, if you don’t make any changes, you’ll no longer see your users’ IP addresses in your origins (and therefore in your logs). Instead, you’ll see the IP addresses of Transparent Edge's servers. This is because the users establish a connection with our servers, so we are the ones making a request to their servers in the case of a MISS. This header is included to give you traceability of end users. Unlike the X-Forwarded-For header, this one sends the end user’s IP without any intermediate proxies.

## geo\_country\_code and geo\_region\_code

For each request, we geolocate the users’ IP addresses using a specific geolocation database. Although it's possible to gather more information using VCL, we take the most common and useful data and include them in header paths available in both VCL and the client's origins, with data about the country's ISO code and the geographical region where the IP is located.

We update the databases weekly, but the accuracy of the region is limited by factors such as the providers’ CGNAT. The country code, however, is highly reliable.

## X-Device

We know that managing user-agents can be a nightmare, which is why we standardize the `User-Agent` header of the original request and create an extra header with three possible values:  `mobile`, `tablet` y `desktop`.

## X-Forwarded-Proto

Despite the growing standardization of HTTPS certificates worldwide, there are still web services that use the HTTP protocol to communicate in plaintext. The X-Forwarded-Proto header—also reflected in logs—has a value of http or https to indicate whether the original request was in plaintext or ciphertext.

## X-Remote-Port

The same as the header `True-Client-IP` , this header is included to give you traceability of the port used by the end users as received by Transparent Edge\`s servers.

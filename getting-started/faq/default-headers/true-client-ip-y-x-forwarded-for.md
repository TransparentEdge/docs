# True-Client-IP and X-Forwarded-For

The client’s IP address is masked by Transparent Edge’s cache network. This means that the client’s origin server is always going to receive connections from an IP address in **our cache network**. Plus, that cache network will vary over time, making it impossible to filter by IP address (level 3 OSI).

To do so, at Transparent Edge we create extra non-standard HTTP headers to be able to send our clients the IP addresses of the actual users who are doing the browsing. This lets clients take the appropriate actions with the addresses even though the server is behind the cache network. This is done using the X-Forwarded-For and/or True-Client-IP headers.

From there, it’s up to the client to capture the header and operate with it.

### What's the difference between X-Forwarded-For and True-Client-IP?

**X-Forwarded-For** is a standard HTTP header where the software of various proxies adds the client’s IP address, including corporate systems. This means that if a request crosses several layers, several addressees will appear in the header separated by commas. As a result, clients will either have to “separate” the last IP address or analyze several of them.

**True-Client-IP** returns the last IP address before the request reaches the Transparent Edge servers. It corresponds to the first IP address of X-Forwarded-For and is therefore the actual address of the client that initiated the connection.

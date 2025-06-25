# Transparent Edge’s IP addresses

As part of our transparency policy, it’s important that the Transparent Edge nodes can be identified at all times, not just by our clients but by any person who wishes to know them.

That is why we publish an endpoint[ ](https://api.transparentcdn.com/v2/companies/ipranges/)in our API with all our public IP addresses: [https://api.transparentcdn.com/v2/companies/ipranges/](https://api.transparentcdn.com/v2/companies/ipranges/).

That endpoint contains all the IP addresses that may connect to your origin/backend, it's extremely important that those IPs are allowed in the origin server(s) firewall, IDS, etc.

The same IPs can also be retrieved in plain format: [https://api.transparentcdn.com/v2/companies/ipranges?format=txt](https://api.transparentcdn.com/v2/companies/ipranges?format=txt)

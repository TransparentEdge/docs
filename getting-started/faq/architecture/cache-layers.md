# Cache layers

Transparent Edge has two cache layers that are optional for our clients. The goal is to improve the[ hit ratio](https://soporte.transparentcdn.com/projects/incidencias/wiki/Hit_Ratio) and reduce the number of unnecessary requests to their origin servers. Plus, this type of architecture also protects against floods of traffic from, for instance, a recursive invalidation of a highly trafficked website.[​![Edit](https://lh3.googleusercontent.com/XVRX59bJfo82y5_tqiBKlBcITZkb6DPNfo4tNbIunmANKyUqH7V1pTl0173uD1ch2lCxzuzp-Tcc7axNaLSTbiCRopH7Fc62ONtRTSVN9lYxRrBjbrIOszoKNzDRaZE3xHpdM8yLhSqaYt1aS3bcLA)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura_de_caches/edit?section=2)

#### Layer 1

Layer 1 is comprised of [globally](https://www.transparentedge.eu/en/) distributed servers, which are what initially attend to end-user requests. They act as SSL terminations and cache servers, serving nearly 95% of Transparent Edge.

These Layer 1 servers can be configured to be the origin of all the Layer 2 servers. That way, when an object needs to be refreshed, the Layer 2 servers send the request directly to the Layer 1 servers. [​![Edit](https://lh3.googleusercontent.com/XVRX59bJfo82y5_tqiBKlBcITZkb6DPNfo4tNbIunmANKyUqH7V1pTl0173uD1ch2lCxzuzp-Tcc7axNaLSTbiCRopH7Fc62ONtRTSVN9lYxRrBjbrIOszoKNzDRaZE3xHpdM8yLhSqaYt1aS3bcLA)](https://soporte.transparentcdn.com/projects/incidencias/wiki/Arquitectura_de_caches/edit?section=3)

#### Layer 2 (Mid-Tier)

Transparent Edge's Mid-Tier is designed to act as a funnel and minimize the number of requests that reach our clients’ origin servers. This server layer is distributed across several data centers to ensure high availability, and it is ultimately in charge of talking with the origin web servers.

To better understand the benefits of this architecture, let’s look at an example:

Suppose we only had one layer of 10 cache servers, all of them talking directly with the origin servers. With a smooth flow of traffic, when an object expires it will do so more or less at the same time in all the caches. So each of the 10 servers would send a request to the origin server to refresh the object.

```
Total: 10 simultaneous requests to the origin.
```

Now suppose we add a second cache layer comprised of two servers. This layer will talk directly with the client's servers and with Layer 1. Layer 1 will still have 10 servers and will only talk with the second layer we just added (funnel).

Under the same circumstances of a smooth traffic flow, an object will expire on all the servers at the same time and the 10 servers will request the object from the second cache layer. This layer only will request the object from the origin twice (once per server) because it can serve the other eight requests without having to go get the object from the client’s origin servers. In this case, we are reducing the traffic at the origin by 80%.

```
Total: 2 simultaneous requests to the origin.
```

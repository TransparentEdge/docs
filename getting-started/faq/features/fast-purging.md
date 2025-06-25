# Fast purging

At Transparent Edge, by default, we use fast purging to instantaneously purge content. This system lets us invalidate content on all Transparent Edge nodes in under two seconds.

#### How do we implement fast purging?&#x20;

It’s quite simple. All Transparent Edge nodes are subscribed to a fanout queue in an internal Rabbit MQ cluster. When it receives a content invalidation message from our[ API](https://docs.transparentedge.eu/v/english/getting-started/faq/glosario/api), the message is immediately consumed by an agent which runs on all our CDN nodes. The agent locally invalidates the requested content at that node.

If you want to find out more about how to invalidate content, click this[ link](https://docs.transparentedge.eu/getting-started/dashboard/invalidando-contenido).

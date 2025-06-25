# Prefetching Cache

The prefetching cache function enables you to request content to be loaded to all edge node caches automatically, and it "warms up" the cache with the requested URLs.

Our dashboard allows you to prefetch for a given resource, i.e., you cache the content on your nodes so that it is immediately available to your clients without having to make the request to the source.

#### From our dashboard

Log in to our dashboard and go to:

Prefetching cache -> New prefetch

Enter the list of URLs you want to prefetch and click on "Prefetch".

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

#### Using our API

Create a POST request to&#x20;

`/v1/companies/{company_id}/prefetch/`

The body must contain a key named "urls", for example:

```
{
  "urls": [
    "https://www.example.com/image.jpg",
    "https://www.example.com/style.css"
  ]
}
```

In case of doubts, please reference our [API documentation](https://api.transparentcdn.com/docs).


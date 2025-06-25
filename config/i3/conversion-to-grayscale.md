# Conversion to grayscale

[i3](./), our image management solution, allows you to dynamically and transparently convert your images to grayscale.&#x20;

To do this, we use our `TCDN-i3-transform` header, specifying the desired operation, which in this case is `grayscale`.&#x20;

For example, if you wanted to serve grayscale images on your domain `mi-dominio.es`, you can simply deploy a [VCL](../vcl/) [configuration](broken-reference) similar to the following from the [dashboard:](../../getting-started/dashboard/)

<pre class="language-c"><code class="lang-c"><strong># i3 - grayscale
</strong>sub vcl_recv {
    if (req.http.host == "www.mi-dominio.es") {
        set req.http.TCDN-i3-transform = "grayscale";
    }
}
</code></pre>

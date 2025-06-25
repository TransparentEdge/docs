# Acting on the Query String

At Transparent Edge, we implement [Varnish Enterprise](https://www.varnish-software.com/es/), and therefore you can take advantage of all the benefits it offers.&#x20;

In this case, we will discuss how to use the [urlplus](https://docs.varnish-software.com/varnish-enterprise/vmods/urlplus/) module in Transparent Edge to interact with the query string of a web request.

### Normalizing the QS

You may want to normalize the URL, particularly its parameters. For example, the order in which they arrive may be important to increase [cache effectiveness](../getting-started/faq/hitratio.md), and you may want to ensure that the URL parameters always arrive in the same order regardless of the site from which they are called

```javascript
sub vcl_recv
{
    urlplus.write();
}
```

In any case, if you don't want to sort it, you just need to do the following:

```javascript
sub vcl_recv
{
    urlplus.write(sort_query = false);
}
```

{% hint style="info" %}
You must always call the "write" method when working with the urlplus module and you want to modify the URL.
{% endhint %}

### We remove parameters from the QS

Sometimes it may be necessary to remove certain parameters from the query string that arrives at Transparent CDN in order to improve cache effectiveness or simply because we want the URL to be sent to the origin without a specific parameter. One way to do this is as follows, in this case, we are removing a parameter named random:

```javascript
sub vlc_recv {
    urlplus.query_delete("random");
    urlplus.write();
}
```

### Removing a parameter from the QS using regex:

Just like the previous example, you may be interested in removing one or multiple parameters from the query string based on a regular expression. For example, we can remove all the parameters added by Google Analytics:

```javascript
sub vcl_recv
{
    urlplus.query_delete_regex("utm_");
    urlplus.write();
}
```

Other functions that can be useful to you are:&#x20;

* **query\_keep.** to remove all the query string except for a specific parameter.
* **query\_get**. to retrieve the value of a parameter from the query string.
* **tolower**. to convert the entire URL to lowercase.
* **toupper**. to convert the entire URL to uppercase.
* **query\_add**. to add a parameter to the URL.

You can find the complete documentation of this [Varnish Enterprise](https://www.varnish-software.com/es/) module at the following [link.](https://docs.varnish-software.com/varnish-enterprise/vmods/urlplus/)

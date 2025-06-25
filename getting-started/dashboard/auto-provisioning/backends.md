# Backends

In this section, you'll be able to view/add/edit or delete backend configurations.

Provisioning -> Backends

<figure><img src="../../../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

A backend (or origin server) is a server identified by an IP address or DNS name. This is where your CDN will fetch your content.

{% hint style="danger" %}
It's extremely important that all the output IPs from the CDN are able to reach the backend server, which means that any firewall or any other protection in your backend must allow for those IPs, which can be queried with [this endpoint](https://api.transparentcdn.com/v2/companies/ipranges).

The actual IP of the client visiting your site will be displayed in the True-Client-IP HTTP header.
{% endhint %}

{% hint style="info" %}
Remember that you don't need to create a new backend per site, and if two or more sites share that same backend then it can be shared.
{% endhint %}

### Backend configuration

The following options are required when creating or editing a backend.

<table><thead><tr><th width="265">Option</th><th width="477">Description</th></tr></thead><tbody><tr><td><strong>Name</strong></td><td>A descriptive and unique name for the backend.</td></tr><tr><td><strong>IP / Origin</strong></td><td>The origin server IP address or DNS name.</td></tr><tr><td><strong>SSL</strong></td><td>True if the connection is encrypted with (HTTPS).</td></tr><tr><td><strong>Port</strong></td><td>Port number where the origin server is listening for http/s connections, usually 80 or 443.</td></tr><tr><td><strong>Healthcheck - Host</strong></td><td>The host header to send to the backend server, for example if your backend server is running Apache, it would be the <code>ServerName</code> or <code>ServerAlias</code> value, for example: <code>www.example.com</code>.</td></tr><tr><td><strong>Healthcheck - URL</strong></td><td>The URL where the healthcheck will send the request. The URL should point to a very lightweight resource that never changes, for example an empty txt file in your webserver.</td></tr><tr><td><strong>Healthcheck - Status code</strong></td><td>The expected status code. If the backend server answers with any other code, the backend will be marked as unhealthy and non-cached content will fail.</td></tr></tbody></table>

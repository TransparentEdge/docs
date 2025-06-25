# Configuration deployments

In this section, we will explain how to manage the configuration logic that applies to your sites in the CDN.

Our CDN uses a modified version of the Varnish Configuration Language (VCL), a powerful language that will enable you to customize your site behavior.

### Visualizing the current state

To view the configuration deployment currently active, navigate to:

Provisioning -> VCL Configs

A table will be displayed with the history of all configurations.

<figure><img src="../../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

The current and active configuration is the one marked with a green tick under the "Status" header. You can see when that configuration took effect under "Deployment date".

You can perform the following actions on any of the configurations:

* **`View`** - Opens a read-only view of the configuration, where you can check the code and compare with other configurations.
* **`Duplicate`** - Opens an editor to deploy a new configuration prefilled with the code of this particular configuration.
* **`Back to production`** - A quick way to deploy an older configuration again, it's a "rollback" button.

### Deploying a new configuration

To deploy a new configuration, click on the "ADD VCL CONFIG" button or, if you want to edit over an older configuration, use the "Duplicate" button instead.

An editor will be displayed where you can edit the VCL code or you can copy it to your favorite editor and paste it later.

The editor embeds some autocompletion that can be activated using the `CTRL` key.

<figure><img src="../../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

When you're done editing the configuration, click on "Save". The configuration will be checked for syntax errors and, if any are present, an error will be shown. For example, here we tried to use `req.http` in the `vcl_backend_response` subroutine which only accepts `bereq.http` and `beresp.http`:

<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

Otherwise, if the code is correct. the deployment will be queued and a clock will be displayed in the status of that configuration. Deployments usually take between 2 and 7 minutes to fully propagate.

{% hint style="info" %}
Remember that everything that you can do on our dashboard is available on our [API](https://api.transparentcdn.com/docs).
{% endhint %}

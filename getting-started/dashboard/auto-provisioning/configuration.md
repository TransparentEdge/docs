# Configuration

Our dashboard provides two configuration methods. The first one, **Easy Setup**, is a simple rule engine that allows you to configure the system without dealing with VCL code. The second option gives you the ability to **write your VCL code**, enabling a more detailed customization of the system’s behavior.

### Easy Setup

The Easy Setup is based on 'if-then' conditional logic. When the conditions of a defined rule are met, a specific action is applied. This system allows you to configure key actions on your sites in a simple and agile way with just a couple of clicks.

To create a new rule, we must select the site where our configuration will be implemented.Once selected, click the '**Add Rule'** button, which will open a modal displaying the rule templates grouped by category.

In '**Cache & Performance'** we can find:

* **Bypass cache:** Select the pages where requests skip the cache and go straight to the origin server.
* **Cache time for CDN:** Specify the TTL for the request.
* **No cache:** Disable caching for requests.

In **Headers & Response** we can see:

* **Add header to the response:** Add a custom header to the HTTP response before it is sent to the user.
* **Header rewrite:** Rewrite any response header before inserting in the cache.
* **Unset header:** Unset an origin header.

In R**outing & Redirects** are:

* **Assing backend:** Assign each site to its corresponding backend for precise and controlled routing.
* **Permanent redirect (301):** Set up a permanent redirect for your website.
* **Temporary redirect (302):** Set up a temporary redirect for your website.
* **Redirect:** Redirects users to another page.
* **Redirect HTTPS:** Redirect HTTP to HTTPS depending on the host name.

In **Security & Access Control** we have:

* **Request blocking:** Block the selected request with an error code.
* **Add exception to the WAF:** A configuration that allows the WAF to bypass specific rules, preventing the blocking of allowed requests or behaviors.
* **Rate limit:** a mechanism that restricts the number of requests a client can make to a server within a given time period.

With the template already selected, we will choose whether the rule will apply to all inbound requests or, alternatively, if we want to define it for a custom filter rule. In the latter case, we must fill in the required values. Once everything is completed, we save, and our new rule will be implemented.

From the rules table, we can edit, delete, and deactivate the rules that have already been created.


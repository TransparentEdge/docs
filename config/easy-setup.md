# Easy setup

The Easy Setup is based on 'if-then' conditional logic. When the conditions of a defined rule are met, a specific action is applied. This system allows you to configure key actions on your sites in a simple and agile way with just a couple of clicks.

To create a new rule, we must select the site where our configuration will be implemented. Once selected, click the '**Add Rule'** button, which will open a modal displaying the rule templates grouped by category.

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-03-19 a las 9.14.23.png" alt=""><figcaption></figcaption></figure>

In **Cache & Performance** we can find:

* **Bypass cache:** Select the pages where requests skip the cache and go straight to the origin server.
* **Cache time for CDN:** Specify the TTL for the request. It specifies, in seconds, the amount of time a response should be kept in cache before requesting it again from the origin.
* **No cache:** Disable caching for requests.

In **Headers & Response** we can see:

* **Add header to the response:** Add a custom header to the HTTP response before it is sent to the user. To do this, you must define the name of the header you want to add and the value it will have.
* **Header rewrite:** Rewrite any response header before inserting in the cache. Define the header name to rewrite and the value it will take.
* **Unset header:** Unset an origin header.

In **Routing & Redirects** are:

* **Assing backend:** Assign each site to its corresponding backend for precise and controlled routing.
* **Permanent redirect (301):** Set up a permanent redirect for your website.
* **Temporary redirect (302):** Set up a temporary redirect for your website.
* **Redirect:** Redirects users to another page.
* **Redirect HTTPS:** Redirect HTTP to HTTPS depending on the host name.

In **Security & Access Control** we have:

* **Request blocking:** Block the selected request with an error code.
* **Add exception to the WAF:** A configuration that allows the WAF to bypass specific rules, preventing the blocking of allowed requests or behaviors. In this field, we must enter the identifier of the rule we want to be ignored.
*   **Rate limit:** a mechanism that restricts the number of requests a client can make to a server within a given time period.

    * _Request:_ Defines how many requests are allowed within the selected time period before the action is triggered.
    * _Time:_ Defines the time period during which requests are accumulated.
    * _Punishment:_ Defines the duration of the penalty applied when a user exceeds the configured limit.
    * _Action:_ Determines the action the CDN takes when the rate limit is exceeded.

    Example:

```
Request: 100
Time: 60s
Punishment: 30s
Action: CAPTCHA
```

This means that a user can make up to 100 requests per minute. If the user exceeds that limit, a CAPTCHA will be displayed for 30 seconds.

*   **WAF mode:** Allows configuring how the WAF should act when handling requests that may represent a threat.

    _Disabled mode (OFF)_ does not analyze or filter requests.

    _Active mode (ON)_ analyzes requests and automatically blocks those that match security rules or malicious patterns. This mode enables taking action in real time against potential attacks.

    Finally, _Detection-only mode (DetectionOnly)_ detects potential threats or suspicious behavior but does not block traffic. This allows reviewing the configuration without affecting real traffic.

In **Other actions** we can find:

* **Ignore QueryString:** This rule ignores the URL parameters (query string) when generating the cache key, treating all requests to the same path as identical.



With the template already selected, we will choose whether the rule will apply to all inbound requests or, alternatively, if we want to define it for a custom filter rule. In the latter case, we can define more specific conditions on the incoming request. For example, **it is possible to check whether the client's IP is included or not in an access control list** (ACL), combining the condition type, the operator, and the name of the list we want to apply.

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-04-28 a las 12.53.35.png" alt="" width="563"><figcaption></figcaption></figure>

Once everything is completed, we save, and our new rule will be implemented.

From the rules table, we can edit, delete, and deactivate the rules that have already been created.

---
description: Mitigate automated threats based on IP address reputation.
---

# Bot Mitigation

Transparent Edge Bot Mitigation is in its core **a curated IP address reputation database that is updated in real time** that protects customer websites from malicious synthetic traffic.

A bot is a software application that runs automated tasks against online services, there are bad bots and good bots.

* **Good bots** are usually web scrapers for the search engines (googlebot, bingbot, ...) and respect [robots.txt](https://en.wikipedia.org/wiki/Robots.txt).
* **Bad bots** search the web with malicious intents, trying to find vulnerabilities to exploit, automating denial of service attacks, sabotage websites ...

Detecting a bot can be a trivial task if it's a simple bot, but advanced bots use an ordinary web engine to scrape, navigate links at random intervals, use the mouse... they become almost humans.



<table><thead><tr><th width="183">Bot Type</th><th>Description</th></tr></thead><tbody><tr><td>Simple</td><td>Connects from a single IP address and uses automated scripts that do not try to impersonate as a browser.</td></tr><tr><td>Moderate</td><td>Uses a headless browser that can even execute javascript.</td></tr><tr><td>Advanced</td><td>Simulates mouse movements and clics, mimicking human behaviour. Uses browser automation technologies. Used by botnets.</td></tr><tr><td>Evasive</td><td>Same as advanced but leverages on VPNs, proxies and other spoofing methodologies to hide.</td></tr></tbody></table>

## Bot Mitigation Settings

If you have adquired the Bot Mitigation service, you'll be able to access its settings where you can customize the threat detection level per site.

The screenshot below shows a small part of the available options:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>A demo of the available settings</p></figcaption></figure>



## How to enable Bot Mitigation

After you've registered a site at the Bot Mitigation settings panel, you can activate an action if a bot is detected according to the established settings.

There are four available actions:

* block
* captcha
* jschallenge
* bypass

{% hint style="info" %}
You can also define a more tailored reaction by using the call [botm assessment](../config/vcl/callable-functions.md#botm-assessment).
{% endhint %}

### Block

For example if you want to protect your site `www.example.com` and you've checked to detect IP addresses categorized as abusive and with a minimum risk score of 60 the following VCL code will block the IP addresses that match those settings:

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "block";
    }
}
```

Of course the condition can be anything you like, perhaps you only want to protect some paths of your website:

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        if (req.url ~ "^/admin") {
            # Only for /admin*
            set req.http.TCDN-BM-Action = "block";
        }
    }
}
```

### JavaScript challenge

Instead of blocking the request directly, you can protect them using a JavaScript challenge that will filter most of the bots and automated request to your site.

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "jschallenge";
    }
}
```

### Captcha

You can also force a captcha for the detected IP addresses, users that successfully complete the captcha will be able to enter your website and the risk of their IP address will decrease overtime.

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "captcha";
    }
}
```

### Bypass

Lastly, maybe you only want the statistics that the detection engine provides automatically without blocking anything, in that case you can use the bypass option.

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "bypass";
    }
}
```

## Enhance the detection

Our database is very effective and evolves in real time, but there is a faster and complementary way to perform bot detection: using our soft fingerprinting script.

It is a small javascript that you can include in your website. The script will run a test using the javascript engine of the user that connects to your website. The test is really fast and checks characteristics of the JS engine, for example the precision of floating point numbers and sends a small report to our API for deep analysis.

You can find the script in our within the Bot Mitigation settings in our [dashboard](https://dashboard.transparentcdn.com/).


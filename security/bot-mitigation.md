---
description: Mitigate automated threats based on IP address reputation.
---

# Bot Management

Transparent Edge Bot Management is in its core **a curated IP address reputation database that is updated in real time** that protects customer websites from malicious synthetic traffic.

A bot is a software application that runs automated tasks against online services, there are bad bots and good bots.

* **Good bots** are usually web scrapers for the search engines (googlebot, bingbot, ...) and respect [robots.txt](https://en.wikipedia.org/wiki/Robots.txt).
* **Bad bots** search the web with malicious intents, trying to find vulnerabilities to exploit, automating denial of service attacks, sabotage websites ...

Detecting a bot can be a trivial task if it's a simple bot, but advanced bots use an ordinary web engine to scrape, navigate links at random intervals, use the mouse... they become almost humans.



<table><thead><tr><th width="183">Bot Type</th><th>Description</th></tr></thead><tbody><tr><td>Simple</td><td>Connects from a single IP address and uses automated scripts that do not try to impersonate as a browser.</td></tr><tr><td>Moderate</td><td>Uses a headless browser that can even execute javascript.</td></tr><tr><td>Advanced</td><td>Simulates mouse movements and clics, mimicking human behaviour. Uses browser automation technologies. Used by botnets.</td></tr><tr><td>Evasive</td><td>Same as advanced but leverages on VPNs, proxies and other spoofing methodologies to hide.</td></tr></tbody></table>

## Bot Management Settings

If you have adquired the Bot Management service, you'll be able to access its settings where you can customize the threat detection level per site.

The screenshot below shows a small part of the available options:

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-05-11 a las 12.20.13.png" alt=""><figcaption><p>A demo of the available settings</p></figcaption></figure>



## How to enable Bot Management

After enabling Bot Management for a site via the Enable button, a set of configurable fields will appear for bot detection.

### **BattleBot**

At the top of the page, the **BattleBot** snippet is displayed — a lightweight JavaScript that can be inserted into the `<head>` of your website to add an extra layer of protection. When a user visits the site, the script runs a fast, transparent test against their browser's JavaScript engine, analysing characteristics such as floating-point number precision. The results are sent to our API for deep analysis, enabling more accurate bot detection on top of IP reputation scoring.

### **Score threshold**

Defines the minimum score at which the configured action is triggered. The range goes from 1 to 99, where lower values imply more aggressive detection with a higher chance of false positives, and higher values result in more conservative detection with fewer false positives. The recommended range is between 75 and 90.

### **Flags**

Flags allow you to fine-tune detection by combining an IP's reputation score with known behavioural categories or characteristics. Any combination of the following flags can be selected:

| Flag                      | Description                                                                                                                                |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **datacenter**            | The IP belongs to a datacenter or cloud provider and is unlikely to correspond to a real residential user.                                 |
| **vpn**                   | The IP is associated with a VPN provider and may be used to mask the user's true location or identity.                                     |
| **proxy**                 | The IP is acting as a proxy server, routing traffic on behalf of other clients.                                                            |
| **tor**                   | The IP is a TOR exit node, commonly used for anonymous browsing and to bypass geo-restrictions.                                            |
| **abuse**                 | The IP has been reported for abusive behaviour such as spam, brute-force attacks, or fraud.                                                |
| **automated\_navigation** | The IP has been associated with automated browser activity, such as headless browsers or bot frameworks.                                   |
| **scraper**               | The IP has been detected harvesting content from websites at scale.                                                                        |
| **ai\_scraper**           | The IP belongs to an AI training crawler or data pipeline scraping content to train machine learning models.                               |
| **botnet**                | The IP is part of a botnet and is likely being used to conduct coordinated malicious activity such as DDoS attacks or credential stuffing. |

#### **Flag threshold**

Only active when at least one flag has been selected. Defines the minimum score an IP must reach for the configured action to be triggered in combination with the selected flags. This value must be lower than the Score threshold.

## Select the action

Once the thresholds and flags have been configured, clicking Save configuration opens a modal to define what happens when a bot is detected.

There are four available actions:

* block
* captcha
* jschallenge
* bypass

{% hint style="info" %}
You can also define a more tailored reaction by using the call [botm assessment](../config/vcl/callable-functions.md#botm-assessment).
{% endhint %}

#### Block

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

#### JavaScript challenge

Instead of blocking the request directly, you can protect them using a JavaScript challenge that will filter most of the bots and automated request to your site.

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "jschallenge";
    }
}
```

#### Captcha

You can also force a captcha for the detected IP addresses, users that successfully complete the captcha will be able to enter your website and the risk of their IP address will decrease overtime.

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "captcha";
    }
}
```

#### Bypass

Lastly, maybe you only want the statistics that the detection engine provides automatically without blocking anything, in that case you can use the bypass option.

```perl
sub vcl_recv {
    # Enable bot mitigation action
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "bypass";
    }
}
```

## **Last step**

Below the action selector, a VCL snippet is provided for advanced use cases. Copy the snippet if you intend to insert it under your own conditions in the VCL editor.

To apply the configuration automatically to all traffic on the site, click **Create VCL automatically**. If you prefer to handle the VCL insertion yourself, click **I'll configure it manually** to copy the snippet and integrate it on your own terms.


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

## What is BotM?

BotM is our Bot Management engine, built directly into our CDN. It analyzes incoming requests in real time and assigns each client IP a **risk score** based on pre-computed threat intelligence data, identifying IPs associated with botnets, scrapers, VPNs, Tor exit nodes, proxies, datacenters, and other sources of automated or abusive traffic.

When a request arrives, BotM:

1. Looks up the client IP in a high-speed local cache.
2. If not cached, queries the threat intelligence backend for an up-to-date score and set of flags for that IP.
3. Compares the score and flags against the thresholds you configured for your site.
4. Takes the configured action (block, challenge, or bypass) if the thresholds are exceeded.

BotM adds minimal latency: lookups are served from cache whenever possible, with tight timeouts on backend calls so it never becomes a bottleneck on your traffic.

### **BattleBot**

At the top of the page, the **BattleBot** snippet is displayed — a lightweight JavaScript that can be inserted into the `<head>` of your website to add an extra layer of protection. When a user visits the site, the script runs a fast, transparent test against their browser's JavaScript engine, analysing characteristics such as floating-point number precision. The results are sent to our API for deep analysis, enabling more accurate bot detection on top of IP reputation scoring.

## Bot Management Settings

If you have adquired the Bot Management service, you'll be able to access its settings where you can customize the threat detection level per site.

The screenshot below shows a small part of the available options:

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-05-11 a las 12.20.13.png" alt=""><figcaption><p>A demo of the available settings</p></figcaption></figure>

After enabling Bot Management for a site via the Enable button, a set of configurable fields will appear for bot detection.

### **Score threshold**

**Range:** 1-99

Every IP has a risk score. Set the score at or above which the configured action fires. IPs scoring below this threshold are allowed through without restriction.

* A **lower threshold** is more aggressive, it catches more IPs but may produce false positives.
* A **higher threshold** is more conservative, only clearly malicious IPs are acted upon.

The recommended range is between 75 and 90.

### **Flags**

Flags allow you to fine-tune detection by combining an IP's reputation score with known behavioural categories or characteristics. Any combination of the following flags can be selected:

| Flag                      | Description                                        |
| ------------------------- | -------------------------------------------------- |
| **datacenter**            | IP originates from a datacenter (non-residential)  |
| **vpn**                   | IP is associated with a VPN service                |
| **proxy**                 | IP is a known open proxy                           |
| **tor**                   | IP is a known Tor exit node                        |
| **abuse**                 | IP has been reported for abusive behavior          |
| **automated\_navigation** | IP has been detected performing automated browsing |
| **scraper**               | IP is associated with scraping activity            |
| **AI scraper**            | IP is associated with AI/LLM content scraping      |
| **botnet**                | IP is part of a known botnet                       |

#### **Flag Score threshold**

**Range:** 1-99 (should be lower than the Score Threshold)

A secondary rule to catch moderate-risk IPs that also exhibit specific suspicious characteristics. The action fires when **both** conditions are true:

* The IP's score is at or above this threshold, **and**
* At least one of the selected flags (see below) matches the IP's profile.

This lets you, for example, block known VPN or Tor IPs at a lower score than you would block a generic risky IP.

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-05-20 a las 13.46.28.png" alt=""><figcaption></figcaption></figure>

### Select the action

Once the thresholds and flags have been configured, clicking Save configuration opens a modal to define what happens when a bot is detected.

There are four available actions:

* **block:** Returns a `403 Forbidden` response immediately.
* **captcha:** Redirects the user to a CAPTCHA challenge. On success, a signed cookie is set and the user continues normally. Only applies to `GET` requests; other methods are blocked with `403` until the captcha is resolved.
* **jschallenge:** Presents a JavaScript-based challenge instead of a CAPTCHA.
* **bypass:** Checks the IP against thresholds but takes no action. The result is available for use in downstream VCL logic (see [Advanced Configuration](../getting-started/dashboard/auto-provisioning/configuration.md)).

{% hint style="info" %}
You can also define a more tailored reaction by using the call [botm assessment](../config/vcl/callable-functions.md#botm-assessment).
{% endhint %}

#### Saving the Configuration

After configuring thresholds, flags, and an action, save and choose how to apply it:

* **Create VCL automatically**: A custom VCL snippet is created and deployed for your site. No further action required.
* **I'll configure it manually**: No VCL is generated. You can copy the snippet created at the top and paste it into the VCL editor. (see [Advanced Configuration](../getting-started/dashboard/auto-provisioning/configuration.md#write-your-vcl-code)).

***

### Advanced Configuration

For scenarios that go beyond what the dashboard supports, you can configure BotM directly in VCL. This overrides any thresholds set in the dashboard. Note that you still need to enable BotM for the site.

#### Basic Usage

Set the `TCDN-BM-Action` header anywhere in `vcl_recv`:

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "jschallenge";
    }
}
```

#### Overriding Thresholds

The full syntax for the header value is:

```
<ACTION>@<SCORE_THRESHOLD>;<FLAG_SCORE_THRESHOLD>;<FLAGS>
```

* `ACTION`: one of `block`, `captcha`, `jschallenge`, or `bypass`
* `SCORE_THRESHOLD`: score at or above which the action triggers (1-99)
* `FLAG_SCORE_THRESHOLD`: score threshold applied when matching flags are present (1-99)
* `FLAGS`: flag codes to check (e.g. `vt` for VPN + Tor), can be empty for no flags, check [BotM Flags API reference](https://api.botm.transparentedge.io/api/report/flags) to see the available flag letter codes

#### Examples

**Different action per country:**

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if (req.http.geo_country_code ~ "^(ES|PT)$") {
            # Higher thresholds and JS challenge for ES/PT traffic
            set req.http.TCDN-BM-Action = "jschallenge@90;70;vt";
        } else {
            # Block everything else that exceeds default thresholds configured in the dashboard
            set req.http.TCDN-BM-Action = "block";
        }
    }
}
```

**Act only on cache miss (bypass mode):**

Use `bypass` to run the assessment without taking immediate action. If the IP exceeds the configured thresholds, the header `tcdn-bm-deny-reason` is set and available in subsequent subroutines such as `vcl_miss`, `vcl_pass` or `vcl_backend_fetch`.

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if (req.url ~ "^/img/") {
            set req.http.TCDN-BM-Action = "bypass";
        }
    }
}

sub vcl_miss {
    # Only present if the IP exceeded the configured thresholds
    if (req.http.tcdn-bm-deny-reason) {
        call deny_request;
    }
}
```


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

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-11-03 a las 13.38.33.png" alt=""><figcaption><p>A demo of the available settings</p></figcaption></figure>

After enabling Bot Management for a site via the Enable button, a set of configurable fields will appear for bot detection.

### **Score threshold**

**Range:** 1-99

Every IP has a risk score. Set the score at or above which the configured action fires. IPs scoring below this threshold are allowed through without restriction.

* A **lower threshold** is more aggressive, it catches more IPs but may produce false positives.
* A **higher threshold** is more conservative, only clearly malicious IPs are acted upon.

The recommended range is between 75 and 90.

### **Flags**

Flags allow you to fine-tune detection by combining an IP's reputation score with known behavioural categories or characteristics. Any combination of the following flags can be selected. The code is the letter used in the [`TCDN-BM-Action` header](bot-management.md#overriding-thresholds).

**IP flags**, characteristics of the client IP. They are checked against the [Flag Score threshold](bot-management.md#flag-score-threshold):

| Flag                      | Code | Description                                        |
| ------------------------- | ---- | -------------------------------------------------- |
| **datacenter**            | `d`  | IP originates from a datacenter (non-residential)  |
| **vpn**                   | `v`  | IP is associated with a VPN service                |
| **proxy**                 | `p`  | IP is a known open proxy                           |
| **tor**                   | `t`  | IP is a known Tor exit node                        |
| **abuse**                 | `a`  | IP has been reported for abusive behavior          |
| **automated\_navigation** | `A`  | IP has been detected performing automated browsing |
| **scraper**               | `s`  | IP is associated with scraping activity            |
| **AI scraper**            | `i`  | IP is associated with AI/LLM content scraping      |
| **botnet**                | `b`  | IP is part of a known botnet                       |

**Request flags**, detected on each request. When one matches, the action applies regardless of the IP score and both thresholds:

| Flag                      | Code | Description                                        |
| ------------------------- | ---- | -------------------------------------------------- |
| **client impersonation**  | `C`  | TLS fingerprint, user agent and headers mismatch   |
| **AI training crawler**   | `T`  | AI crawler collecting content to train models      |
| **AI search crawler**     | `S`  | AI crawler indexing content for AI search results  |
| **AI user fetcher**       | `U`  | AI assistant fetching a page a user asked for      |

Blocking `S` removes the site from those AI search results, and blocking `U` stops users from reading it through that assistant. The current list of flags is also available as JSON in the [BotM Flags API](https://api.botm.transparentedge.io/api/report/flags).

#### **Flag Score threshold**

**Range:** 1-99, usually lower than the Score Threshold. Each threshold is checked on its own.

A secondary rule to catch moderate-risk IPs that also exhibit specific suspicious characteristics. The action fires when **both** conditions are true:

* The IP's score is at or above this threshold, **and**
* At least one of the selected IP flags (see above) matches the IP's profile.

This lets you, for example, block known VPN or Tor IPs at a lower score than you would block a generic risky IP.

<figure><img src="../.gitbook/assets/Captura de pantalla 2026-11-03 a las 13.39.05.png" alt="" width="563"><figcaption></figcaption></figure>

### Select the action

Once the thresholds and flags have been configured, clicking Save configuration opens a modal to define what happens when a bot is detected.

The available actions are:

* **block:** Returns a `403 Forbidden` response immediately.
* **captcha:** Shows a CAPTCHA challenge on the same page. On success, a signed cookie is set and the page reloads, the user continues normally. Only applies to `GET` requests; other methods are blocked with `403` until the captcha is resolved.
* **pow\_captcha:** Same as `captcha`, but the browser first solves a JavaScript challenge and only then the CAPTCHA is shown.
* **jschallenge:** Presents a JavaScript-based challenge instead of a CAPTCHA.
* **bypass:** Checks the IP against thresholds but takes no action. The result is available for use in downstream VCL logic (see [Advanced Configuration](bot-management.md#advanced-configuration)).

#### Saving the Configuration

After configuring thresholds, flags, and an action, save and choose how to apply it:

* **Create VCL automatically**: A custom VCL snippet is created and deployed for your site. No further action required.
* **I'll configure it manually**: No VCL is generated. You can copy the snippet created at the top and paste it into the VCL editor. (see Advanced Configuration).

***

### Advanced Configuration

For scenarios that go beyond what the dashboard supports, you can run BotM directly from your VCL with the `protect_with_botm` subroutine. BotM must still be enabled for the site in the dashboard.

#### Usage

Call `protect_with_botm` where you want BotM to run. The action, and optionally the thresholds, are taken from the `TCDN-BM-Action` header:

* **No header:** `block` with the thresholds configured in the dashboard.
* **`<ACTION>`:** that action with the thresholds configured in the dashboard.
* **`<ACTION>@<THRESHOLDS>`:** that action with the thresholds set in VCL (see [Overriding Thresholds](bot-management.md#overriding-thresholds)).

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "jschallenge";
        call protect_with_botm;
    }
}
```

How it works:

* BotM runs **at the point of the call**. If the IP exceeds the thresholds, the action is applied right there: `block`, `captcha`, `pow_captcha` and `jschallenge` end the request (unless the visitor already solved the challenge), `bypass` lets it continue.
* It can be called from `vcl_recv`, `vcl_miss` and `vcl_pass`.
* Each call uses the `TCDN-BM-Action` header and removes it, so you can call it several times in the same request with different actions or thresholds. The first call that blocks or shows a challenge ends the request.
* BotM does not run on `OPTIONS` requests, ESI subrequests and restarted requests, and never acts on allow-listed IPs (see [Global Whitelists](global-whitelists.md)). The exception is allow-listed AI crawlers when the AI crawler flags (`T`, `S`, `U`) are configured: the action applies to them, and they are no longer treated as allow-listed for the rest of the request.
* `TCDN-BM-Action` and `tcdn-bm-deny-reason` headers sent by clients are removed before your VCL runs.

`ACTION` is one of `block`, `captcha`, `pow_captcha`, `jschallenge` or `bypass`.

#### Overriding Thresholds

The full syntax for the `TCDN-BM-Action` header value is:

```
<ACTION>@<SCORE_THRESHOLD>;<FLAG_SCORE_THRESHOLD>;<FLAGS>
```

* `ACTION`: one of `block`, `captcha`, `pow_captcha`, `jschallenge` or `bypass`
* `SCORE_THRESHOLD`: score at or above which the action triggers (1-99)
* `FLAG_SCORE_THRESHOLD`: score threshold applied when matching flags are present (1-99), usually lower than `SCORE_THRESHOLD`. Each threshold is checked on its own.
* `FLAGS`: flag codes to check (e.g. `vt` for VPN + Tor), can be empty for no flags. Codes are listed in [Flags](bot-management.md#flags)

#### Examples

**Different action per country:**

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if (req.http.geo_country_code ~ "^(ES|PT)$") {
            # Higher thresholds and JS challenge for ES/PT traffic
            set req.http.TCDN-BM-Action = "jschallenge@90;70;vt";
        } else {
            # Block everything else that exceeds the thresholds configured in the dashboard
            set req.http.TCDN-BM-Action = "block";
        }
        call protect_with_botm;
    }
}
```

**Different score threshold per flag (several calls):**

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        # VPN or Tor from score 60, any IP from score 95
        set req.http.TCDN-BM-Action = "block@95;60;vt";
        call protect_with_botm;

        # Datacenter from score 85
        set req.http.TCDN-BM-Action = "block@95;85;d";
        call protect_with_botm;
    }
}
```

**Stricter rules for a path:**

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        if (req.url ~ "^/login") {
            set req.http.TCDN-BM-Action = "captcha@70;50;vtpd";
        } else {
            set req.http.TCDN-BM-Action = "jschallenge";
        }
        call protect_with_botm;
    }
}
```

**Act only on cache miss:**

Call it from `vcl_miss` and `vcl_pass` so cache hits skip the check entirely and only requests going to the origin are assessed. Set the header right before the call, in the same subroutine:

```vcl
sub vcl_miss {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "block";
        call protect_with_botm;
    }
}

sub vcl_pass {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "block";
        call protect_with_botm;
    }
}
```

{% hint style="warning" %}
Do not set `TCDN-BM-Action` in `vcl_recv` if you call `protect_with_botm` from `vcl_miss` or `vcl_pass`: a header left in `vcl_recv` is applied at the end of `vcl_recv` (see below), on every request, including cache hits.
{% endhint %}

**Use the result in your own logic (`bypass`):**

With `bypass`, no action is taken. If the IP exceeds the thresholds, the header `tcdn-bm-deny-reason` is set and is available in the rest of your VCL, including `vcl_miss`, `vcl_pass` and `vcl_backend_fetch`. Each call sets or removes it, so it holds the result of the last call:

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com" && req.url ~ "^/img/") {
        set req.http.TCDN-BM-Action = "bypass";
        call protect_with_botm;
    }
}

sub vcl_miss {
    # Only present if the IP exceeded the thresholds
    if (req.http.tcdn-bm-deny-reason) {
        call deny_request;
    }
}
```

#### `TCDN-BM-Action` Header Without the Call

Setting the `TCDN-BM-Action` header without calling `protect_with_botm` is still supported: if the header is still present at the end of `vcl_recv`, BotM runs at that point with that action.

```vcl
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-BM-Action = "jschallenge";
    }
}
```

{% hint style="warning" %}
With this method BotM does not run where the header is set, but after all your `vcl_recv` code:

* If your `vcl_recv` ends earlier, for example with `return (pass);`, BotM does not run at all.
* If the header is set several times, only the last value is used.
* It runs on every request, including cache hits.

Use `call protect_with_botm;` instead.
{% endhint %}

# Wordpress plugin

**Transparent Edge Cache** is the official WordPress plugin for the Transparent Edge CDN platform. Unlike generic plugins such as WP Rocket or W3 Total Cache, it's built specifically to take advantage of the exclusive Varnish Enterprise features that Transparent Edge offers.

With this plugin your WordPress site:

* Serves pages from the CDN cache instantly
* Updates automatically when you publish or edit content
* Pre-warms purged pages so no visitor ever gets a slow page
* Optimizes images on the fly without touching your server
* Reduces the weight of CSS, JavaScript and HTML for faster loading

#### What makes it different from other cache plugins?

Traditional cache plugins don't know about Transparent Edge's capabilities. This plugin uses features no other plugin supports:

<table data-search="false"><thead><tr><th width="350.3828125">Feature</th><th width="118.5">WP Rocket</th><th width="99.91015625">W3TC</th><th width="110.37890625">TE Cache</th></tr></thead><tbody><tr><td>Surgical invalidation (Surrogate-Keys)</td><td>No</td><td>No</td><td>Yes</td></tr><tr><td>Soft Purge (zero downtime)</td><td>No</td><td>No</td><td>Yes</td></tr><tr><td>Automatic pre-warming</td><td>No</td><td>No</td><td>Yes</td></tr><tr><td>Image optimization on the CDN (i3)</td><td>No</td><td>No</td><td>Yes</td></tr><tr><td>Tag-based invalidation (not URL-based)</td><td>No</td><td>No</td><td>Yes</td></tr><tr><td>CSS/JS minification + Combine</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Lazy Load + LCP Preload</td><td>Yes</td><td>Partial</td><td>Yes</td></tr><tr><td>Object Cache (Redis)</td><td>No</td><td>Yes</td><td>Yes</td></tr></tbody></table>

{% hint style="info" icon="lightbulb-on" %}
If you're currently using WP Rocket or W3 Total Cache, disable them before activating Transparent Edge Cache. This plugin replaces all of their functionality.
{% endhint %}

### Installation

#### Prerequisites

* WordPress 5.5 or higher (6.0+ recommended)
* PHP 7.4 or higher (8.1+ recommended)
* A Transparent Edge account with API access (Company ID, Client ID, Client Secret)
* Your site must be configured to route through the Transparent Edge CDN

{% hint style="info" %}
API credentials are found in your Transparent Edge dashboard: **Settings → API**. If you don't have them, contact your account manager.
{% endhint %}

#### Installation steps

1. Download the `transparent-edge-cache.zip` file
2. In WordPress, go to **Plugins → Add New → Upload Plugin**
3. Select the ZIP file and click **Install Now**
4. Click **Activate Plugin**
5. A new **TE Cache** menu appears in the left sidebar

#### Quick setup wizard

The first time you open the plugin, a setup wizard walks you through the whole process:

**Step 1 — Site type.** The plugin automatically scans your install and detects the type of site (blog, corporate, online store with WooCommerce, membership), along with language plugins (WPML, Polylang), page builders (Elementor, Divi) or SEO plugins (Yoast, Rank Math). You can confirm the detection or change it manually.

**Step 2 — Credentials.** Enter your Company ID, Client ID and Client Secret. The plugin verifies the connection to the Transparent Edge API.

**"Connect and Configure" button.** With one click, the plugin verifies the credentials, applies the optimal configuration for your site type, enables the recommended features, and is ready to go.

{% hint style="info" %}
If you'd rather set everything up manually, click "Skip wizard, configure manually."
{% endhint %}

### Dashboard

The Dashboard is the first tab you see inside TE Cache. It gives you an at-a-glance status of every feature:

* **CDN Status** — shows whether the plugin is connected to the Transparent Edge API and displays the current TTL configuration. **✓ Active** in green means everything is working correctly.
* **Object Cache** — shows whether object caching is enabled (Redis or APCu) and its hit ratio. A high ratio (>90%) means most database queries are served from memory.
* **Minification Cache** — number of CSS and JS files the plugin has minified and stored on disk. Clear it with the **"Clear Minify Cache"** button.
* **WPO Features** — how many web performance optimizations are active out of those available.
* **Invalidation History** — direct link to the Transparent Edge dashboard showing every purge event: what was purged, when, and with what result.

### Connection

This tab is where you configure API credentials and find the quick actions.

#### API credentials

You'll need three values, available in your Transparent Edge dashboard (**Settings → API**):

* **Company ID** — your company's numeric identifier in Transparent Edge
* **Client ID** — your application's API identifier
* **Client Secret** — the secret key used to authenticate

Once entered, click **"Test Connection"** to confirm they're correct.

#### Quick actions

**Purge All Cache** — sends an order to Transparent Edge to remove all cached content for your site. Use it after large-scale changes (theme update, content migration). In normal use you shouldn't need it — the plugin purges what's needed automatically.

{% hint style="warning" %}
After a Purge All, the following visits will be slower until the cache rebuilds. If Preload via Sitemap is enabled, the plugin will pre-warm the pages automatically.
{% endhint %}

**Pre-warm Cache via Sitemap** — crawls your site's sitemap (Yoast, Rank Math or native WordPress) and visits every URL so Varnish caches it. Runs in the background with real-time progress, and can be stopped at any time. It processes 3 URLs every 3 seconds to avoid overloading your server; a 100-page site pre-warms in about 2 minutes.

### Cache configuration

Controls how long content stays cached, both on the CDN and in the visitor's browser.

#### What is TTL?

TTL (**Time To Live**) is how long content stays in cache before it's considered expired. There are two types:

* **s-maxage (CDN TTL)** — how long Varnish keeps a copy of the page. While it's valid, visits are served instantly from the CDN.
* **max-age (Browser TTL)** — how long the visitor's browser keeps a local copy, without even checking the CDN.

#### Dynamic content (HTML)

HTML pages are dynamic content generated by WordPress; the plugin sends cache headers directly from PHP.

<table><thead><tr><th width="207.30078125">Parameter</th><th width="177">Default value</th><th>Explanation</th></tr></thead><tbody><tr><td>CDN TTL (s-maxage)</td><td>172,800s (48 hours)</td><td>Varnish keeps the page for 48h. When new content is published, the plugin automatically purges the affected pages.</td></tr><tr><td>Browser TTL (max-age)</td><td>3,600s (1 hour)</td><td>The visitor's browser serves its local copy for 1h, reducing requests to the CDN.</td></tr></tbody></table>

{% hint style="info" icon="lightbulb-on" %}
You can raise the CDN TTL without worry: when new content is published, the plugin automatically invalidates only the affected pages.
{% endhint %}

#### Static content (CSS, JS, images, fonts)

Static files don't go through PHP. The plugin sets their cache headers depending on your web server:

* **Apache/LiteSpeed** — the plugin writes rules automatically into `.htaccess`
* **Nginx** — the plugin generates a config snippet you copy into your `server {}` block

<table><thead><tr><th width="204.80078125">Parameter</th><th width="187.296875">Default value</th><th>Explanation</th></tr></thead><tbody><tr><td>CDN TTL (s-maxage)</td><td>2,592,000s (30 days)</td><td>Static assets rarely change; 30 days is safe because WordPress versions CSS/JS (<code>?ver=X</code>).</td></tr><tr><td>Browser TTL (max-age)</td><td>86,400s (1 day)</td><td>The browser caches static assets for one day.</td></tr></tbody></table>

Static asset invalidation is automatic: when you upload an image or update a theme/plugin, the plugin purges those files in Varnish without any action on your part.

#### Surrogate-Keys

This is the plugin's most powerful feature. Every page is tagged with unique identifiers (Surrogate-Keys) describing its content — for example, the homepage might carry the tags `front-page`, `post-123`, `type-post`, `author-1`, `term-5`, `tax-category`.

When you publish a new post in the "Sports" category (`term-5`), the plugin tells Varnish: "purge every page tagged `term-5`." Only the pages showing that category get purged; the rest of the site keeps serving from cache untouched.

{% hint style="info" icon="lightbulb-on" %}
This matters a lot for large sites: a news outlet with 50,000 cached pages doesn't need to purge all of them every time it publishes a story — only the 5-10 pages that actually display it.
{% endhint %}

#### Vary by device and language

* **Vary by Device** — enable this if your theme shows different content on mobile and desktop; Varnish will keep two versions of each page.
* **Vary by Language** — enable this if you use WPML or Polylang; Varnish will keep one version of each page per language.

### Automatic invalidation

The plugin automatically detects when your content changes and purges only what's needed in the CDN.

<table data-search="false"><thead><tr><th width="160.24609375">WordPress event</th><th width="390.453125">What gets purged in Varnish</th><th>Pre-warms?</th></tr></thead><tbody><tr><td>Publish or edit a post</td><td>The post + homepage + categories + tags + author</td><td>Yes</td></tr><tr><td>New approved comment</td><td>Only the post's page</td><td>Yes</td></tr><tr><td>Edit a menu</td><td>Pages using that menu + homepage</td><td>Yes</td></tr><tr><td>Edit a widget</td><td>Pages using that sidebar</td><td>Yes</td></tr><tr><td>Switch theme</td><td>The whole site (Purge All)</td><td>Optional</td></tr><tr><td>Upload or edit an image</td><td>The file's URL + thumbnails</td><td>No</td></tr><tr><td>Update a theme or plugin</td><td>All CSS and JS</td><td>No</td></tr></tbody></table>

{% hint style="warning" %}
Only a theme switch triggers a Purge All. Every other event is surgical: it purges only the pages containing the changed content.
{% endhint %}

#### Purge methods

* **Soft Purge (recommended)** — marks content as stale in Varnish but keeps serving it while a fresh copy is fetched from origin. The visitor never experiences downtime. If your server temporarily fails, Varnish keeps serving the last valid version.
* **Hard Purge** — removes content immediately; the next request goes to origin. Use it only when you need the change to be instant and don't mind a possible MISS.

#### Automatic pre-warming (Warm-up)

After every purge, the plugin automatically visits the purged pages so Varnish caches them again, so no real visitor ever gets an uncached page. Example, publishing in "Sports":

1. The plugin sends a `tag_invalidate` to Varnish with the affected tags
2. Varnish purges the homepage, the Sports archive, the feed…
3. The plugin does a GET on those pages
4. Varnish goes to origin, generates the fresh pages, and caches them
5. The next visitor gets a HIT with the story already visible

{% hint style="info" icon="lightbulb-on" %}
Pre-warming is capped at 20 URLs per event to avoid overloading the origin. For a full Purge All, use Preload via Sitemap.
{% endhint %}

#### Preload via Sitemap

When this option is enabled, after a Purge All the plugin automatically crawls your sitemap (Yoast, Rank Math or native WordPress) and pre-warms every URL in the background, without blocking your work.

### Image optimization (i3)

i3 is Transparent Edge's image optimization service: it transforms images at the CDN edge, without touching your server or modifying the original files.

* **Auto WebP** — automatically converts JPEG and PNG images to WebP for supporting browsers (25-50% lighter)
* **Quality control** — adjusts JPEG and WebP quality (80% by default, a good balance of size and visual quality)
* **Size limit** — set a maximum size (e.g. 500KB); images are progressively compressed to stay under it

Enabled from the "i3 Images" tab. The plugin generates a VCL snippet you need to deploy from your Transparent Edge dashboard (**Configuration → VCL**), copying it with the "Copy VCL" button.

{% hint style="warning" %}
The VCL snippet includes a domain filter so it doesn't affect other sites on your account. Don't modify the snippet unless you know what you're doing.
{% endhint %}

### Frontend optimization (WPO)

Optimizations that improve Core Web Vitals:

* **CSS minification** — strips comments and whitespace; cached to disk; already-minified `.min.css` files are skipped
* **Combine CSS** — merges local CSS files into a single file, cutting down HTTP requests; external and conditional CSS is preserved
* **JS minification** — more conservative than CSS: strips block comments and extra whitespace while preserving code structure; jQuery and already-minified files are skipped
* **Combine JS** — only combines footer scripts, for safety; jQuery and WordPress core scripts stay separate
* **Defer JS** — adds the `defer` attribute so scripts don't block HTML parsing, improving LCP
* **Delay JS** — scripts don't load at all until the user interacts with the page; improves INP
* **Lazy Load for images and iframes** — defers off-screen content until scroll brings it into view; the first 2 images load immediately to avoid hurting LCP
* **Preload LCP** — detects the page's main image and adds a preload hint in the `<head>`
* **Self-host Google Fonts** — serves fonts from your own domain: improves TTFB, is GDPR-friendly, and is cached for 30 days
* **Automatic DNS Prefetch** — detects the third-party domains your site uses and adds DNS prefetch/preconnect hints
* **Remove Unused CSS** — generates, per template, a version of the CSS containing only the rules that template actually uses; computed in the background and never breaks your site's appearance if generation fails
* **Lazy Load for CSS background images** — defers CSS-defined background images until they're about to enter the viewport; the first 2 stay active

{% hint style="warning" %}
If Delay JS breaks something, add the affected script to the exclusion list. jQuery is always excluded automatically.
{% endhint %}

Delay JS, Defer JS and Combine JS respect WordPress's script dependency tree, avoiding errors like "Backbone is not defined" or issues with forms that depend on libraries like Backbone or Underscore, with no manual configuration needed.

### Speculation Rules (prefetch and prerender)

A modern standard that lets the browser load pages ahead of time that the visitor is likely to visit next, so pages appear almost instantly on click. The plugin generates these rules automatically, with smart exclusions so it never preloads cart, checkout, account area, forms, etc.

#### Operating modes

* **Conservative** — only preloads on click or mouse-down. The lowest load on origin; recommended for high-traffic sites.
* **Balanced** — preloads on link hover. A good balance between speed and load; recommended for most sites.
* **Aggressive** — preloads on hover and prerenders (fully renders) on click. The fastest option, but it executes JavaScript on speculated pages, which can trigger analytics or remarketing pixels before the actual visit. Requires explicit confirmation to enable.

You can choose which content types get preloaded (posts, pages, products, etc.).

The header can be injected via PHP (origin, no extra config needed) or via VCL (edge, more efficient — the plugin generates the recommended snippet). Either way, the plugin never deploys VCL automatically: it only generates the recommendation for you to apply.

If it detects another prefetch/prerender plugin active (WordPress Speculative Loading, Flying Pages, WP Rocket's Preload Links, etc.), it warns you to avoid duplicate preloading.

### Security

Combines protections that work on any WordPress install with Transparent Edge's own security services.

At the top of this tab you'll see which security services you have with Transparent Edge (WAF, Bot Mitigation, Perimetrical, Anti-DDoS), detected automatically from your account. Features that require active protection (rate limiting, anti-bot, automatic anomaly response) require the Perimetrical suite and are managed from the Transparent Edge platform, not from the plugin.

#### Local hardening

Available to all customers, regardless of contracted security services:

* **Block PHP in uploads** — prevents PHP files from executing inside `/wp-content/uploads/`, a common attack vector. Applied automatically via `.htaccess` on Apache; on Nginx the equivalent rule is shown.
* **Disable XML-RPC** — disables `xmlrpc.php`, unless you use Jetpack or the WordPress mobile app.
* **Limit login attempts** — temporarily blocks an IP after several failed login attempts, mitigating brute-force attacks.

#### Security headers

The plugin helps you configure the main HTTP security headers (Content-Security-Policy, Strict-Transport-Security, X-Content-Type-Options, X-Frame-Options, Referrer-Policy and Permissions-Policy), generating a recommended VCL snippet you apply from your Transparent Edge panel — or, alternatively, issuing them from PHP.

The CSP header starts by default in **Report-Only** mode: it logs violations without blocking anything, so you can test it safely before enforcing it.

### Advanced configuration

* **Excluded URLs** — URLs that should never be cached; one pattern per line, regex supported (e.g. `/my-private-page/`, `/api/`)
* **Excluded cookies** — if a visitor has one of these cookies, the page isn't cached; WordPress and WooCommerce cookies are already included by default
* **Ignored query strings** — tracking parameters (UTM, fbclid, gclid, etc.) create duplicate cache entries; the plugin generates a VCL snippet that strips them from the cache key using `urlplus`
* **Heartbeat control** — WordPress sends AJAX requests every 15-60 seconds for autosave and post locking. You can reduce the frequency (60-120s), disable it in the admin, or disable it in the editor too (which also disables autosave — use with caution)

### WooCommerce

If you have WooCommerce installed, the plugin integrates automatically.

#### **Pages excluded from cache:**&#x20;

* **Cart** — contains data unique to each visitor, so it must not be cached
* **Checkout** — a personalized payment process
* **My Account** — the customer's personal data
* **Order confirmation page** — order-specific data

#### Product invalidation:

<table><thead><tr><th width="244.171875">Event</th><th>What gets purged</th></tr></thead><tbody><tr><td>Edit a product</td><td>Product page + categories + shop + homepage</td></tr><tr><td>Stock change</td><td>Product page + categories</td></tr><tr><td>Order completed/cancelled</td><td>All products in the order</td></tr><tr><td>Product review</td><td>Product page</td></tr><tr><td>Scheduled sales (start/end)</td><td>Shop + on-sale products</td></tr></tbody></table>

### Object Cache

Object Cache stores database query results in memory (Redis or APCu), drastically cutting the number of SQL queries WordPress needs to render a page.

A typical WordPress site runs 100-300 database queries per page; with Object Cache, most are served from memory in microseconds. This especially helps when Varnish gets a MISS and needs to go to origin.

To enable it: go to the plugin's Dashboard — if Redis or APCu are available on your server, you'll see an "Activate" button.

{% hint style="info" icon="lightbulb-on" %}
Redis requires the PHP redis extension to be installed on your server. If it isn't available, contact your hosting provider.
{% endhint %}

### WordPress Multisite

If your install is a multisite network, the plugin supports network activation with:

* **Shared credentials** — configure API credentials once for the whole network; individual sites inherit them
* **Force credentials** — optionally prevent individual sites from changing them
* **Auto-activation** — new sites created in the network are configured automatically with the network settings
* **Network overview** — from Network Admin → TE Cache you can see the status of every site
* **Purge All Network** — button to purge the cache of every site in the network at once

### Troubleshooting

| Problem                                | Possible cause                                                               | Solution                                                                                             |
| -------------------------------------- | ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Pages always show MISS                 | The site isn't routed through the CDN, or active session cookies are present | Verify the domain points to the CDN. Test as an anonymous visitor (logged out).                      |
| Fonts broken after enabling Minify CSS | The minification cache has stale files                                       | Go to the Dashboard and click "Clear Minify Cache," then run a Purge All.                            |
| Purge doesn't work                     | Incorrect or expired credentials                                             | Check the credentials in the Connection tab and review the invalidation history in the TE dashboard. |
| Delay JS breaks the site               | A critical script is being delayed                                           | Add the affected script to Delay JS's exclusion list (jQuery is already excluded).                   |
| Object Cache unavailable               | Redis or APCu aren't installed                                               | Contact your hosting provider to install the PHP Redis extension.                                    |
| Combine JS breaks functionality        | A script depends on being loaded in a specific order                         | Add that script's handle to Combine JS's exclusion list.                                             |
| Browser shows old content              | Browser TTL (max-age) is too high                                            | Do a hard refresh (Ctrl+Shift+R). Optionally, lower the HTML max-age.                                |
| Static assets aren't cached in Varnish | Headers aren't reaching Varnish (Nginx)                                      | On Nginx, copy and apply the config snippet shown by the plugin in the Cache tab.                    |

### Glossary

| Term           | Meaning                                                                                                                   |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- |
| CDN            | Content Delivery Network. A network of distributed servers that serve your content from the point closest to the visitor. |
| Varnish        | High-performance HTTP cache software used by Transparent Edge to store and serve your pages.                              |
| TTL            | Time To Live. How long content stays in cache before expiring.                                                            |
| s-maxage       | HTTP header indicating the TTL for intermediate caches (like Varnish). Doesn't affect the visitor's browser.              |
| max-age        | HTTP header indicating the TTL for the visitor's browser.                                                                 |
| HIT            | When Varnish has a valid cached copy and serves it directly. Fast.                                                        |
| MISS           | When Varnish has no copy (or it expired) and needs to go to the origin server. Slower.                                    |
| Soft Purge     | Invalidation method that marks content as stale but keeps serving it while a fresh copy is fetched                        |
| Surrogate-Keys | Tags identifying a page's content to allow surgical invalidation.                                                         |
| Warm-up        | Process of pre-warming the cache by visiting pages so Varnish caches them before real visitors arrive.                    |
| WPO            | Web Performance Optimization. A set of techniques to make a website load faster.                                          |
| LCP            | Largest Contentful Paint. Google metric measuring when the page's main content appears.                                   |
| INP            | Interaction to Next Paint. Google metric measuring how long a page takes to respond to user input.                        |
| VCL            | Varnish Configuration Language. Language used to configure Varnish's behavior.                                            |
| i3             | Transparent Edge's image optimization service that transforms images at the CDN edge.                                     |
| Object Cache   | In-memory cache (Redis/APCu) for WordPress database query results.                                                        |
| Drop-in        | A special PHP file that WordPress loads automatically to replace core functionality (e.g. `object-cache.php`).            |

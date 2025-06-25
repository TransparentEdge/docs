# Wordpress plugin

Before the installation

To properly configure the plugin, there are some interesting wp-config.php considerations before activating the cache.

#### Debug parameters

`define( 'WP_DEBUG', false );`

`define( 'WP_DEBUG_DISPLAY', true );`

`define( 'WP_DEBUG_LOG', false );`

`define( 'SAVEQUERIES', false );`

`define( 'SCRIPT_DEBUG', false );`

#### Limiting revisions

It is interesting to limit the number of saves we make of our pages and posts in the database:

`define( 'WP_POST_REVISIONS', 50 );`

If we are going to activate the Cron Job, we can remove the native WordPress cron:

`define( 'DISABLE_WP_CRON', true );`

We define a high autosave interval if it is a CMS with many editors, so as not to saturate the source:

`define( 'AUTOSAVE_INTERVAL', 360 );`

In case we want to limit the PHP limit:

`define( 'WP_MEMORY_LIMIT', '256M' );`

Self-emptying of the recycle garbage can so as not to saturate the database:

`define( 'EMPTY_TRASH_DAYS', 7 );`

We limit plugin editing and installation, and updates:

`define( 'DISALLOW_FILE_EDIT', false );`

`define( 'DISALLOW_FILE_MODS', true );`

`define( 'WP_AUTO_UPDATE_CORE', false );`

If we do not have redirect, we force SSL on login and backend, to avoid problems:

`define( 'FORCE_SSL_LOGIN', true );`

`define( 'FORCE_SSL_ADMIN', true );`

## Plugin installation

We install the plugin as you would install any WordPress plugin:

<figure><img src="https://lh6.googleusercontent.com/_lTJfmnrsfkd5JVbS9LLAdLlRxZk81Z-IPrfT__sEsgbxuAHZLNuES_luafksqx7aZydg8eN-xVR4OfGYQjvh7lJSyARsli8ejvMgIE05FBLmp4jbRnGTdq6lL1ccdIDrDTWackRB50dk0kRwJTJero" alt=""><figcaption></figcaption></figure>

Once the plugin is installed as standard we configure the total cache in pro mode in the `wp-config.php` with: `define( 'W3TC_PRO', true )`;

After that we can move on to the configuration part within the WordPress backoffice, we start with the 'Configuration Guide', a submenu of the main menu of the plugin: 'Performance'.

<figure><img src="../.gitbook/assets/wordpress-1.jpg" alt=""><figcaption></figcaption></figure>

This Wizard will allow us to test the basic caches before even registering the CDN and see what we have available on the machine (object cache, query cache, etc.).

<figure><img src="https://lh5.googleusercontent.com/48kx5125D-R93Sn0-6NNOoX_P4gmBJt554hzUMIc2nNJGRScDbE8TOw9hLM3NpSBmC9YiTsMOVF13VTNFeHik8OJi900B7hXEK6ZGpiEP7Qxc8KuyLkwAaUUtzqNhma7ybhyuF5IgI-zQVVr_PJl2FU" alt=""><figcaption></figcaption></figure>

We start with the page cache, obtaining different results depending on the origin. When in doubt, 'Disk: Enhanced' is best.

<figure><img src="https://lh5.googleusercontent.com/3BZjr8SmhKOYLZgIqj5O0DxnKvStLvzRkgym3iBRG3vDqW8QeipYJdullv830gxAqFLZ-MdIrWtXDv-_WB99VwbYgY6iuah16T_zSF_TagMc7r0tBjeuAjTJdv-wY4oFJG3k4jS7E11pPpnd3-mmMHU" alt=""><figcaption></figcaption></figure>

We continue with the database. In this case it is important to know if we have Memcached or Redis available, which would normally be the fastest options.

<figure><img src="https://lh4.googleusercontent.com/7pPdnq65l5DAGjLbkf8_TT7WZ9ngR8_XVC6za5h7EvwJZ8s-e9dKjTybxFa3u_QPe7xn4dfO7HBVr7SebAg75qmdF-8SwWlZ38rLE8nNh7jz0T0P5g2Oea1h6Y930klnmYBXcsW46twPGPbWN5y-Slg" alt=""><figcaption></figcaption></figure>

In the object cache, exactly the same, we will choose the one that best suits us from the options that we have installed in the source machine.

<figure><img src="https://lh3.googleusercontent.com/vwXE3LGQoVfo_AoXIqxz8xXDPd2HV7UALwVJxLqQtAo_VXOgLWYTlz7DRTjXz6JQUdFTb2SOk9hLstalzO13cJfYf9XnjVWQ9tg4BZeNZkAnyunQQF40c9F4rC7i-wiy79EK9tXGyGbxSspZYmZ5ohE" alt=""><figcaption></figcaption></figure>

Then we continue with one of the most important: the browser cache test, where we will see if we can get cache control with the `max-age` and `s-maxage` headers.

<figure><img src="https://lh6.googleusercontent.com/O4zvJbb6flgpUXvcyacvC-KRSlchI2kQEGDJS1iieXUeLO9qKe28Ddd9rKtLfuoVV7lPKEdbQi20pkD77LPvuNA0jWVpiViD0RWXcC_tcB70S3SgPgLfyREy7DmhfCdi9qIyAAW2sepHvcYTrkjJmJU" alt=""><figcaption></figcaption></figure>

We continue with the Wizard in the part of **lazy load** or deferred loading of images, which will help us a lot. Here we must be cautious with the options that the WordPress theme itself incorporates so as not to compete with it.

<figure><img src="https://lh6.googleusercontent.com/DT-CA-M6zrQKdkhtITrImh-zPrSQYcxzsKbEmoUk4MznvWj_qoXi26mvOV3MtpTg3t9ZRgJHSCSxewhS2xkxa0a3FeqOS6WQdB_ycRoRANmd4BT6VOe0U9Q5IMz4iX27xZAk1nsmxuaSjDZo9Z6panI" alt=""><figcaption></figcaption></figure>

Once the setup has been completed and the basic features have been activated, we will go to the 'Show features' submenu, where we can install and review the basic features installed.

<figure><img src="https://lh5.googleusercontent.com/ZS1IQLNaUMxFKRCE_2wL0O3TGxR8oyqQ-0UUJNDD9djqyRZPkT-OnxWb0ZXvlKgJ0n_177nM-JoA38Js3V4gookeNWL8lZMB1KFZB_UQMHXhBT682b2IeLT35wfLjAiiuEEkXBKzBeF3TE4cnM6VFho" alt=""><figcaption></figcaption></figure>

The PRO version of the plugin will allow us to install other available options that may be useful to us depending on the power of our template, layout builder, etc. This can be done in the 'Extensions' menu. Here we can activate or deactivate the ones we consider useful or not and adjust their values.

<figure><img src="https://lh5.googleusercontent.com/vl7gxamRI8rtQ0-DQjRqsOk41xnJmy7sQIvalixqeOz4PF7iLMlbkjbRDIl3eq1xTk0FB2WXYd_ONwdviVC4tDL8miSA6SlfNmYlWDfKqwwbrC0uryptXmc7LwXUrYVG4ip7r9elgHPLfjReV87ogPc" alt=""><figcaption></figcaption></figure>

Once this is done, we register the Transparent Edge CDN and configure it in 'General Settings', being able to touch there all the values previously configured in the Wizard. We will find these options by activating the 'FSD CDN' checkbox when selecting 'TransparentCDN'.

<figure><img src="https://lh5.googleusercontent.com/az9NZi3Uy51Lzv9cWem3WiehEhRwkWk53WHPaIIjApqpmMWZ9E_kHUihzj5QH2f1p4Aeh1BsUdxsham_CmvWMOJ6nfeOZD96OgCtEwd5svJTgKGZ0mDZs6x7n9KM-jbonGl-Y-oR-urO_Y_dFyIvOZY" alt=""><figcaption></figcaption></figure>

Then we will add the company ID, client ID and client secret in the CDN menu - Configuration: Full-Site Delivery.

<figure><img src="https://lh6.googleusercontent.com/MtEZa0jZRWsnvDhmNlnKYSHIVig0aRxR1BBUHVd04OOrvA1ALeO_Z51JOJhvO5Ep34fJWdMFXBxeYAcE3o7pp2451JOpy8gT7RgqZrriro5XzUpwp6qLXrtpuuqRYkPorIQi5s5a_Z8OD9wz65o5h_U" alt=""><figcaption></figcaption></figure>

Once this is done, and if the 'Test Transparent CDN' check returns an OK, we could activate the values we want in 'General Settings'. Each activation will allow us to configure separately its values in its corresponding submenus.

Special attention should be paid to the options: minimize, object cache, browser cache and exclusions.&#x20;

Some examples of page cache configuration:

<figure><img src="../.gitbook/assets/worpress-2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh3.googleusercontent.com/ngR8__EIdrspxR-FgLJbRMovZhx__gUnYzLIYyt0rn2aG1Mo1tOsZO7-gYoYgKH-UTOPDX4dFdTi5G61GEbfJdhlOUgwYiJTF2Ky0cdk7wPwhiGNuguzWr4dl9gGurLnkD6WK-meh1vKV0R8Ytl7-6s" alt=""><figcaption></figcaption></figure>

* Particularly interesting here is the option to cache posts, home pages, feeds, 404, and the options to not cache pages for logged in users and/or by roles.&#x20;
* It is very useful to preload cache via sitemap, although it should be activated with some care depending on the WP source.&#x20;
* We can also set the post purge policy, the type of feed to purge, etc.&#x20;
* Here we can also cache the API, an aspect that is very useful in the case of WordPress that do only "back" and have a decoupled front end.&#x20;
* We can set a more or less aggressive garbage collection interval or set the comment cookie, for example, very useful for a site with "forums".&#x20;
* We can define specific user-agents that we do not want to cache pages, or not cache pages with a specific cookie.&#x20;
* We can also exclude pages from caching, complete categories and tags, custom fields, authors, etc.
* In the same way, we can continue configuring the most interesting options, such as minify.

<figure><img src="https://lh3.googleusercontent.com/IQ59iO7PFnc69CfIOUYZxuNfOx2mPPwSagHXYvUjdDU7cwt1-7amsLwvfo-c_8dx-unGC9eRyVxB0S0SkJcW7bQ_MM-fecstQkNTEAui8UW9StnJIWO3jc_fygY7yMlreaUUMQc9L6Il5XQfaMFfZ2M" alt=""><figcaption></figcaption></figure>

* Or the user's browser cache.

<figure><img src="../.gitbook/assets/wordpress-3.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/worpress-4.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/wordpress-5.png" alt=""><figcaption></figcaption></figure>

In any case, and as there are too many settings, we have prepared a plugin configuration file, which can be activated from 'General Settings > Import/Export Settings'. Download your configuration file [here](https://chat.transparentedge.eu/file-upload/dJxdRHgaK2migZA6W/w3totalcache.json?download).

<figure><img src="https://lh3.googleusercontent.com/Bs0MkdEjULUJ7luhP2OVNyJSFOEJNcuihEscS0dRA5fuafP6dF9wfNrBbBO_kmvoTor4Lve1EBiFt1YoV99GwG2k_LNjyyKYyTk0qxAy5odhNnfu5MvWnAsmVPyRYR5vww3KCf6Euck6BDTd4fBDEdc" alt=""><figcaption></figcaption></figure>

With that we would have a starting point with many preconfigured options, where we would obviously need to review that they are suitable for our environment and configure the CDN client IDs with Transparent Edge, in addition to reviewing the options to see if they fit our objectives and cache policies.

# Caching static vs. dynamic objects

We understand static objects to be all objects requested from a server via HTTP or HTTPS protocol that don’t change after they are created. Examples of static objects include images, texts, compressed files (ZIP, TGZ, etc.), style sheets (CSS), JavaScript, XML, etc.

We understand dynamic content to be just the opposite: requests made from a client to a server via HTTP or HTTPS protocol that change with every user iteration. Examples of dynamic content include a bank transaction, a personalization in the HTML of a website, or an online shopping process.

At Transparent Edge, we can cache both static and dynamic content. However, the latter greatly depends on the type of website and how often it is updated.

Let’s take the example of an e-commerce site that has a private user area in the upper-right menu, where a welcome message appears. When you log into the site, you see a page adapted to your shopping preferences. What many e-commerce and similar sites with user personalization do is directly not cache in this area, setting a cache header of zero seconds or sending no-cache in the Cache-Control.

At Transparent Edge, it’s possible to cache all that content if the origin can use a cookie or another header to identify whether or not the user is logged in. So, we could save a cached version of the page for users who aren’t logged in (anonymous browsing) and a different version of the page for every one of the different users who are logged in.

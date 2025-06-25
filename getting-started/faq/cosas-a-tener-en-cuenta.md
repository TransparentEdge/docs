---
description: >-
  There are several things to keep in mind when working with a CDN in front of
  your web service. These are the most important ones:
---

# Things to consider

## Updating content

When you work with a CDN, be aware that there is an element in front of your website which is caching all the content you publish. If you want to update an element on the page, once you have it how you want it, you must be sure to delete the cached content from the CDN cache to make way for the new content.

The process of deleting that content is known as invalidating, and there are several options for doing so.

The first and easiest option is to go to the **Invalidations** section of our[ portal](https://dashboard.transparentcdn.com/auth/login?redirect=%2F) following an update, click the Purge URL button, and enter the URL you wish to invalidate.

![](<../../.gitbook/assets/Captura de pantalla 2020-05-21 a las 9.59.08.png>)

Another option is to use the API to integrate the invalidation process into the publication system. This way, your system will automatically detect when you change content and launch an invalidation through our [API](https://api.transparentcdn.com/docs).

If you are using[ WordPress](https://docs.transparentcdn.com/integraciones/configuracion-plugin),[ Magento](https://docs.transparentedge.eu/integraciones/plugin-para-magento), or[ Prestashop](https://docs.transparentedge.eu/integraciones/plugin-para-prestashop), we have plugins available that are specifically geared toward integrating the CMS with Transparent Edge so you don’t have to worry about it.

## Client IP

When you are behind our CDN, you won’t see the client IP address in the usual way. This is because Transparent Edge is making the requests to the origin servers on the client's behalf. But don't worry, all is not lost.

To deal with this situation, Transparent Edge has created extra HTTP headers that allow us to send customers the IP address of the real user who is doing the browsing. So, even though the server is behind the cache network, the customer can still take the appropriate actions based on the real IP address. We do this with X-Forwarded-For and/or True-Client-IP headers.

We explain it in detail[ here](https://docs.transparentedge.eu/v/english/getting-started/faq/cabeceras-por-defecto/true-client-ip-y-x-forwarded-for).

## IPS/IDS Systems

If you have IPS/IDS systems on your origin platform, you need to be extremely careful. Even though these systems are intended to protect your platform, they can cause problems when working behind a CDN.

One of the many benefits of IPS systems is that they can detect when a particular IP address is accessing a resource illicitly.

As mentioned above, when you are behind a CDN, all the requests to your origin server will come from the CDN's IP addresses. So, the IPS may think that the CDN's IP addresses are making more requests than any one IP should make and restrict them as if it were an attack.

This is obviously a false positive that can be corrected, but it’s something to keep in mind.

At Transparent Edge we publish our [IP](https://api.transparentcdn.com/v1/companies/ipranges/) ranges through our API. If you have these types of systems, we recommend including all the Transparent Edge IP addresses on a whitelist within the IPS.

## Root domains

With Transparent Edge—like with many other CDNs—the most common way to start running the service is by making a simple DNS change so that, for instance, the www subdomain of your website will no longer be an A record but a CNAME record that we’ll give you when you sign up.

So far, so good. But if your website runs directly with the canonical domain—that is, without the www or any other domain (e.g.: **https://mysite.com** instead of **htttps://www.mysite.com**), you’re not done quite yet:

* If your domain doesn’t have any MX records added to it, in theory you’ll be able to use the CNAME of the canonical domain, although not all DNS providers support this option. We repeat: This is the easiest option, but it’s not available with all DNS providers.
* Another option would be to let us take care of the domain management within our DNS system.

## Set-Cookies and pages with basic authentication

Web pages whose responses have the Set-Cookies or Authorization headers are cached in Transparent Edge by default.

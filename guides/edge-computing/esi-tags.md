# ESI Tags

Edge Side Include (ESI) is a markup language developed by Akamai and proposed as a standard to the W3C, but it currently remains a draft. Nevertheless, Transparent Edge Services supports some of the available ESI tags.&#x20;

ESI involves embedding ESI tags within HTML so that those calls are made independently as parallel requests.

```
<HTML>
<BODY>
The time is: <esi:include src="/cgi-bin/date.cgi"/>
at this very moment.
</BODY>
</HTML>
```

In the example above, which could be the index.html of a page, with a cache time of, for example, 3600s (1h), we are making an include within the main HTML that will be treated as a parallel call with its independent cache time from the main page.&#x20;

This way, the call to date.cgi does not have cache, while the rest of the page does. As mentioned earlier, Transparent Edge supports only the \<esi:include> and \<esi:remove> tags.

```
<esi:remove>
  <a href="http://soporte.transparentcdn.com"> Enlace alternativo cuando ESI no funcione</a>
</esi:remove>
```

This functionality is included by default in Transparent Edge with the service price, but it needs to be activated for each site.&#x20;

To do this, go to the [dashboard](https://dashboard.transparentcdn.com/auth/login?redirect=%2F) and click on the Provisioning - VCL Configs tab. Then, switch to advanced mode and duplicate the last active configuration, adding a code similar to this but adapting the domain name:

```javascript
sub vcl_backend_response {
        if (bereq.http.host ~ ".*.transparentcdn.com") {
                set beresp.do_esi = true;
                unset beresp.http.ETag;
                unset beresp.http.Last-Modified;
        }
}
```

Once this is done and the new configuration is deployed, you will be able to serve ESI from Transparent Edge.

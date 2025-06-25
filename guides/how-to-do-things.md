# How to do things

## **How to create a basic configuration**

The minimal configuration to start working on Transparent Edge is to assign a backend in the VCL configuration like this:



```
sub vcl_recv {
    set req.backend_hint = cNNN_example.backend();  
}
```

The above configuration applies to all registered websites. If you want to assign a specific backend to a particular site, you can use this configuration:



```
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.backend_hint = cNNN_example.backend();
    }
}
```

If you also want to cache all static content, you can enhance the configuration with this code:



```
sub vcl_backend_response {    
    if (bereq.http.host == "www.example.com")  {
        if (urlplus.get_extension() ~ "(?i)(jpg|jpeg|png|gif|css)") {
            set beresp.http.Cache-Control = "max-age=86400";
            set beresp.ttl = 86400s;
        }
    }
}
```

##

## &#x20;**How to assign a backend**

```
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.backend_hint = cNNN_example.backend();
    }
}
```

##

## **How to cache all static content on my website**

If you want to cache both in the browser and the CDN:

```
sub vcl_backend_response {    
    if (bereq.http.host == "www.example.com")  {
        if (urlplus.get_extension() ~ "(?i)(jpg|jpeg|png|gif|css)") {
            set beresp.http.Cache-Control = "max-age=86400";
            set beresp.ttl = 86400s;
        }
    }
}  
```

If you want to cache only in the CDN and not in the browser, we use the cache header max-age that affects both browsers and CDN by setting a max-age of zero, then we use the s-maxage header that only affects the CDN to rewrite the behavior only in CDN:



```
sub vcl_backend_response {    
    if (bereq.http.host == "www.example.com") {
        if (urlplus.get_extension() ~ "(?i)(jpg|jpeg|png|gif|css)") {
            set beresp.http.Cache-Control = "max-age=0, s-maxage=86400";
            set beresp.ttl = 86400s;
        }
    }
}
```

## &#x20;

## **How to rewrite origin cache policies on my website**

Example of static cache policies with exceptions and assignment if there is no origin policy

```
sub vcl_backend_response {
    # Assignment of the cache policy for static resources
    if (beresp.status == 200) {
        if (urlplus.get_extension() ~ "(?i)(css|eot|gif|ico|jpe?g|js|png|svg|ttf|woff)" ||
                beresp.http.Content-Type ~ "image/"                                     || 
                beresp.http.Content-Type ~ "text/(css|plain)"                           || 
                beresp.http.Content-Type ~ "(application|text)/(x-)?javascript"         || 
                beresp.http.Content-Type ~ "font/") {
            # Define cache times if they do not exist at the origin
            if (!beresp.http.Cache-Control) {
                set beresp.http.Cache-Control = "max-age=3600, s-maxage=86400";
                set beresp.ttl = 86400s;
            }
            cookieplus.setcookie_delete_regex(".*");
            cookieplus.setcookie_write();
        }
        # Example of lower cache times for html
        if (urlplus.get_extension() ~ "(?i)(html?)"                                    || 
                beresp.http.Content-Type ~ "text/html") {
            set beresp.http.Cache-Control = "max-age=300, s-maxage=3600";
            set beresp.ttl = 300s;
        }
    }
    # No-cache path exceptions
    if (bereq.url ~ "^/path/no-cache")  {
        set beresp.http.Cache-Control = "no-cache";
        set beresp.ttl = 0s;
        set beresp.uncacheable = true;
    }
}
```



## **How to enable waf in detection mode**

Example of WAF in Detection mode

```
sub vcl_recv {
    if (req.http.host == "www.my-domain.com") {
        set req.http.TCDN-WAF-Enabled = "true";
        set req.http.TCDN-WAF-Set-SecRuleEngine = "#DetectionOnly";
    }
}
```



## **How to add WAF exceptions**

Example of WAF with exceptions

```
sub vcl_recv {
    if (req.http.host == "www.my-domain.com") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/without/waf/") {
            set req.http.TCDN-WAF-Set-SecRuleEngine = "#Off";
        }
        if (req.url ~ "^/path/with/exceptions/") {
            set req.http.TCDN-WAF-Allow-Rule-Exceptions = "ruleID_1 ruleID_2 ruleID_3 ... ruleID_n";
        }
    }
}
```



## **How to disable  WAF for a part of my website**

Exclude paths from WAF analysis

```
sub vcl_recv {
    if (req.http.host == "www.my-domain.com") {
        set req.http.TCDN-WAF-Enabled = "true";
        if (req.url ~ "^/path/without/waf/") {
            set req.http.TCDN-WAF-Set-SecRuleEngine = "#Off";
        }
    }
}
```



## **How to enable I3 to transform images to webp and avif formats**

i3 - auto\_webp

```
sub vcl_recv {
    if (req.http.host == "www.my-domain.com") {
        set req.http.TCDN-i3-transform = "auto_webp";
    }
}
```



## **How to enable Bot Mitigation**

First, we need to obtain a token and enable the service for the desired site in the panel in its corresponding section:

Then we configure the appropriate parameters for our site

Once this is done, we can deploy our VCL



To enable Bot mitigation:

```
sub vcl_recv {
    if (req.http.host == "www.mydomain.com") {
        # bypass, block, or challenge
        set req.http.TCDN-BM-Action = "bypass";
    }
}
```

##

## **How to remove parameters from the query string**

There are several ways to remove parameters from a query string. If you want to remove a specific parameter, you can use something like this:

```
sub vcl_recv {
    urlplus.query_delete("random");
    urlplus.write();
}
```



If you prefer, you can also use regex

```
sub vcl_recv {
    urlplus.query_delete_regex("utm_");
    urlplus.write();
}
```



If you need to remove all parameters so they cannot bypass the cache, you can use this:

```
sub vcl_recv {
    urlplus.query_delete_regex(".*");
    urlplus.write();
}
```



## **How to apply a rate limit**

If you want to apply a rate limit to your website based on the client's IP address. This configuration allows 20 requests in 1 second. If that rate is exceeded, the IP is blocked for 30 seconds

```
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-WAF-Set-RateLimit-Key = req.http.True-Client-IP;
        set req.http.TCDN-WAF-Set-RateLimit-Options = "20:1s:30s";
        call rate_limit;
    }
}
```



The rate limit can be applied in combination with a captcha or a javascript challenge, for example:

```
sub vcl_recv {
    if (req.http.host == "www.example.com") {
        set req.http.TCDN-WAF-Set-RateLimit-Key = req.http.True-Client-IP;
        set req.http.TCDN-WAF-Set-RateLimit-Options = "20:1s:30s:captcha";
        # set req.http.TCDN-WAF-Set-RateLimit-Options = "20:1s:30s:js_challenge";
        call rate_limit;
    }
}
```

In this other example, a rate limit is enabled for the UAM so that it only applies to IPs that exceed the Rate Limit.  Here, UAM will be activated for 30 seconds for the site if it receives more than 200 requests in 1 second.

```
if (req.http.host == "www.example.com") {
    set req.http.TCDN-UAM-Activation-RateLimit-Key = req.http.True-Client-IP;
    set req.http.TCDN-UAM-Activation-RateLimit-Options = "20:1s:30s";
    call under_attack;
}
```

##

## **How to enable UAM (Under Attack Mode) from VCL**

You can enable the under attack mode (UAM) selectively from the VCL and apply it only to certain users, for example:

```
if (req.http.host == "www.example.com") {
    if (req.http.geo_country_code == "RU") {
        call under_attack;
    }
}
```



## **How to configure a website with authenticated origin in S3 (AWS)**

If you have an authenticated origin in AWS in S3, you can use the following configuration to configure Transparent Edge in front of the bucket:

```
sub vcl_backend_fetch {
    if (bereq.http.host == "www.example.com") {
        set bereq.http.TCDN-AWS-Access-Key = "A*************Z";
        set bereq.http.TCDN-AWS-Secret-Key = "A*******************Z";
        set bereq.http.TCDN-AWS-Service = "s3";
        set bereq.http.TCDN-AWS-Region = "eu-west-1";
        set bereq.http.host = "example.com.eu-west-1.amazonaws.com";
        set bereq.backend = cNNN_exampleS3.backend();
    }
}
```

The TCDN-AWS-Access-Key and TCDN-AWS-Secret-Key headers are mandatory. The rest of the headers, if not provided, will use the default values which are s3 for Service and us-east-1 for Region. The bereq.http.host header is important to set it with the same value that S3 has configured to handle external calls.

##

## **How to enable midtier for a site**

```
if (req.http.host == "www.example.com") {
    set req.http.TCDN-Midtier-Enabled = "true";
}
```



## How to invalidate contentect from python

Below is a simple example in Python of how to invalidate content on our platform.

The first thing we need to do is to retrieve the token, for which we require the client\_id and client\_secret that can be obtained from our [dashboard](https://dashboard.transparentcdn.com/user/profile).

```python
def get_token():
    # Checking for valid token file
    try:
        st = os.stat(token_file)
        ts_now = int(datetime.timestamp(datetime.now()))
        file_ctime=st.st_ctime
        # If token is not older than 24h
        if ((ts_now - file_ctime) < 86400):
            print(f"Retrieving token from file")
            file = open(token_file, 'r')
            tk = file.read().rstrip('\n')
            file.close()
            return str(tk)
        else:
            pass
    except FileNotFoundError as e:
        print(f"Token file not found: {e}")


    # If token is older than 24h we get a new one from API.
    endpoint = f"https://api.transparentcdn.com/v1/oauth2/access_token/"
    token_req_payload = {'grant_type': 'client_credentials'}
    try:
        resp = requests.post(endpoint, verify=True, allow_redirects=False, data=token_req_payload, auth=(client_id, client_secret))
        tk = json.loads(resp.text)['access_token']
    except Exception as e:
        print(e)
        exit()

    if resp.status_code != 200:
        print(f"Error in token generation: {str(resp.status_code)} - {str(tk)}")
    else:
        print(f"Token emited: {str(tk)}")
        file = open(token_file, 'w')
        file.write(tk)
        file.close()
    return str(tk)

```



Next, once the token is obtained, we proceed to perform the invalidation:

```python
def invalidate_content(token, company_id, urls, ban=False):
    url = f"https://api.transparentcdn.com/v1/companies/{company_id}/invalidate/"
    headers = {
        'Authorization': f'Bearer {token}',
        'Content-Type': 'application/json'
    }
    data = {
        "urls": urls
    }
    if ban:
        data["ban"] = "0" # PURGE Method

    response = requests.post(url, headers=headers, data=json.dumps(data))
    return response.json()

```

Here is the complete script on how to implement this:

```python
#!/usr/bin/env python

import json
import requests
import os
from datetime import datetime

client_id = "***************"
client_secret = "*****************"
token_file="/tmp/token.dat"

# Llamadas al api de Transparent Edge
def get_token():
    # Comprobamos si tenemos un token reciente
    try:
        st = os.stat(token_file)
        ts_now = int(datetime.timestamp(datetime.now()))
        file_ctime=st.st_ctime
        # Si el token tiene menos de 6h lo damos por válido
        if ((ts_now - file_ctime) < 86400):
            print(f"Retrieving token from file")
            file = open(token_file, 'r')
            tk = file.read().rstrip('\n')
            file.close()
            return str(tk)
        else:
            pass
    except FileNotFoundError as e:
        print(f"Token file not found: {e}")


    # Si el token guardado no existe o ha caducado sacamos uno nuevo
    endpoint = f"https://api.transparentcdn.com/v1/oauth2/access_token/"
    token_req_payload = {'grant_type': 'client_credentials'}
    try:
        resp = requests.post(endpoint, verify=True, allow_redirects=False, data=token_req_payload, auth=(client_id, client_secret))
        tk = json.loads(resp.text)['access_token']
    except Exception as e:
        print(e)
        exit()

    if resp.status_code != 200:
        print(f"Error generando el token: {str(resp.status_code)} - {str(tk)}")
    else:
        print(f"Token emitido con éxito: {str(tk)}")
        file = open(token_file, 'w')
        file.write(tk)
        file.close()
    return str(tk)




def invalidate_content(token, company_id, urls, ban=False):
    url = f"https://api.transparentcdn.com/v1/companies/{company_id}/invalidate/"
    headers = {
        'Authorization': f'Bearer {token}',
        'Content-Type': 'application/json'
    }
    data = {
        "urls": urls
    }
    if ban:
        data["ban"] = "1"

    response = requests.post(url, headers=headers, data=json.dumps(data))
    return response.json()

# Ejemplo de uso
token = get_token()
company_id = 82
urls = ["http://www.transparentedge.eu/", "https://www.transparentedge.eu/wp-content/themes/transparent-edge/assets/img/home-decorative-1.png"]
response = invalidate_content(token, company_id, urls)
print(response)
```

**Please, be sure to replace the client\_id and client\_secret fields with yours.**



## **How to apply time restrictions**

f you want, you can set restrictions on a specific resource, whether it is the entire website or a specific URL within that site. For this, you can use the `now` function. This function returns a string in the following format:

```
Thu, 10 Sep 2020 12:34:54 GMT
```

The best way to establish a restriction based on a date is to include the current date within an HTTP header, and once inside, you can apply whatever logic you want.

Here’s a somewhat simple but effective example.



```
if (req.http.host == "www.example.com" && req.url ~ "/restricted_url.html") {
    set req.http.mydate = now;
    if (req.http.mydate ~ " 17:") {
        call deny_request;
    }
}

```

This code prevents access to that URL at 5 PM UTC. This example is quite basic, and you can definitely improve it, but it serves perfectly to explain the use of time restrictions.

{% hint style="info" %}
You need to keep in mind that our platform is in UTC.


{% endhint %}



Perhaps, a more elegant way to do it is by using “utils.time\_format()”. For example, if we wanted to allow access from 18UTC to 20UTC in Spain, we can get the hour with `%H`:

```
sub vcl_recv {
    if (req.http.host == "www.example.com" && req.url ~ "^/restricted_utl.html") {
        if (utils.time_format("%H") ~ "^18|19|20$" && req.http.geo_country_code != "ES") {
            # The request was made between 18 and 20 UTC and is NOT coming from Spain
            call deny_request;
        }
    }
}
```

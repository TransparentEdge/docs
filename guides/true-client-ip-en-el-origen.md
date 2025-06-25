# True-Client-IP in the origin

### How can I log the True-Client-IP header in Apache logs?

It is a relatively simple step, and it is done by including the True-Client-IP header within the "LogFormat" directive in the Apache configuration file (usually httpd.conf). For example, let's say you have enabled logs in the "combined" format, and the original format you have looks like this:

```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
```

To have the True-Client-IP address appear in your logs, once you have enabled the header in the Transparent CDN configuration, you can change it to something like this:

```
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%{True-Client-IP}i\" %q" combine
```

### How to see the real client IP address from your PHP application?

As it is an HTTP header, all the request headers are available in the $\_SERVER variable in PHP. Therefore, you can retrieve the real IP address of the connecting user using $\_SERVER\['True-Client-IP'].

Alternatively, if you cannot or find it complicated to modify the PHP code, you can use the **auto\_prepend\_file** directive in the php.ini file. This directive allows you to execute a PHP script before any other script execution, so it should be a lightweight script. For the specific case at hand, you can do something like this:

```
  ...
  auto_preprend_file = /usr/local/bin/set_ip.php
  ...
```

```
  if (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
      $_SERVER['REMOTE_ADDR'] = $_SERVER['HTTP_X_FORWARDED_FOR'];
  }
```

In this way, if the application uses the $\_SERVER\['REMOTE\_ADDR'] header, you don't need to modify any code because we are already setting the client's IP address as what arrives in the x-forwarded-for header.

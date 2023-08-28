---
title: nginx forward http to https requets
layout: post
---

In `/etc/nginx/nginx.conf`, the normal section for http protocol would look like

```
  server {
    listen       80;
    listen       [::]:80;
    server_name  <server_name>;
    root         /usr/share/nginx/html;

    include /etc/nginx/default.d/*.conf;

    error_page 404 /404.html;
    location = /404.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
  }
```

If one'd like to forward all http request to https request, change it to

```
  server {
    if ($host = <server_name>) {
        return 301 https://$host$request_uri;
    }

    listen       80;
    listen       [::]:80;
    server_name  <server_name>;
    return 404;
  }
```

replacing `<server_name>` with the actual server name.

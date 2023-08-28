---
title: nginx forward http to https requets
layout: post
---

In `/etc/nginx/nginx.conf`, change the section for http protocol to

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

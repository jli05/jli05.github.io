---
title: Migrate a web server with SSL certificates
layout: post
---

## Introduction
Suppose the domain is `example.com`, associated with a static IP address, for which one had generated a SSL certficate before. We will migrate to a new server, at the end of the process, for the same domain name `example.com`.

## A Plan for Migration
1. In [AWS](https://aws.amazon.com), create a new Virtual Machine (VM) instance. Set it up with code and data.
2. Dissociate the Elastic IP from the old server, associate the Elastic IP to the new server even when both are running.
3. To set up the web server, if using the `certbot` ACME client of [Let's Encrypt](https://letsencrypt.org) see below, or with the `acme.sh` client follow [this post](/2023/08/28/acme-SSL-certificates.html).
4. Leave the new server run for some time. Deactivate gradually the crontab jobs on the old server.

## Re-issue the SSL Certificate
To retrieve an already issued certificate with `certbot`, the http (port number 80) port must be open,

```sh
sudo certbot certonly --standalone -d <domain>
```

In `/etc/nginx/nginx.conf`, make sure there are `server` sections under the `http` section.

```
http {
    ....

    server {
        ....

        listen [::]:443 ssl ipv6only=on; # managed by Certbot
        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    }

    server {
        if ($host = tsterm.com) {
            return 301 https://$host$request_uri;
        } # managed by Certbot

        listen       80;
        listen       [::]:80;
        server_name  <server_name>;
        return 404; # managed by Certbot
    }
}
```

in which the referred `/etc/letsencrypt/options-ssl-nginx.conf`,

```
# This file contains important security parameters. If you modify this file
# manually, Certbot will be unable to automatically provide future security
# updates. Instead, Certbot will print and log an error message with a path to
# the up-to-date file that you will need to refer to when manually updating
# this file.

ssl_session_cache shared:le_nginx_SSL:10m;
ssl_session_timeout 1440m;
ssl_session_tickets off;

ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers off;

ssl_ciphers "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";
```

the referred `/etc/letsencrypt/ssl-dhparams.pem`,

```
-----BEGIN DH PARAMETERS-----
...
...
-----END DH PARAMETERS-----
```

At last reload the web server,

on systemd-based systems,

```sh
nginx -t
sudo service nginx force-reload
```

or on [Alpine Linux](https://www.alpinelinux.org),

```sh
/etc/init.d/nginx checkconfig
/etc/init.d/nginx reload
```

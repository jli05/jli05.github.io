---
title: Use acme.sh to install SSL certificates
layout: post
---

## Introduction
For personal experience, [Let's Encrypt](https://letsencrypt.org/) is the best SSL Certificate Authority (CA) so far that issues SSL certificates for free to websites, and is registration-free.

The CA communicates with any client website via [Automatic Certificate Management Environment](https://en.wikipedia.org/wiki/Automatic_Certificate_Management_Environment) protocol. The recommended client program that handles this communication is `certbot`, as per Let's Encrypt.

A prerequisite to installing `certbot` is to install `snapd`, by Let's Encrypt's instruction. On Amazon Linux 2023, we'd be stuck, as `snapd` is not in the software repository.


## `acme.sh`
An alternative client program could be [acme.sh](https://acme.sh), an open-source shell script.

Suppose the web server is [nginx](https://nginx.org). For best ease of installation, we need be as a superuser.

```sh
sudo su
curl https://get.acme.sh | sh -s email=<email>
/root/.acme.sh/acme.sh --set-default-ca --server letsencrypt
/root/.acme.sh/acme.sh --issue --nginx /etc/nginx/nginx.conf -d <server_name>
```

After issuance of the certificate, press `Ctrl+D` to quit the `su` session and return as normal user. Now go to `/etc/nginx/nginx.conf`, uncomment the lines for the https protocol and set the section up.

```
  server {
    listen       443 ssl http2;
    listen       [::]:443 ssl http2;
    server_name  <server_name>;
    root         /usr/share/nginx/html;

    ssl_certificate /root/.acme.sh/<server_name>_ecc/fullchain.cer
    ssl_certificate_key /root/.acme.sh/<server_name>_ecc/<server_name>.key
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers PROFILE=SYSTEM;
    ssl_prefer_server_ciphers on;

    ...
  }
```

Once done, reload the nginx service,

```sh
sudo service nginx force-reload
```

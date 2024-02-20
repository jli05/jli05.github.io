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

## Re-issue the SSL Certificate
To retrieve an already issued certificate with `certbot`, the http (port number 80) port must be open,

```sh
sudo certbot certonly --standalone -d <domain>
```

In `/etc/nginx/nginx.conf`, make sure the SSL parameters exist either under a `http` section or `http { server }` section.

```

```

in which the referred ``,

```

```

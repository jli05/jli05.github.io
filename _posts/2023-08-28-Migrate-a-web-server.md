---
title: Migrate a web server with SSL certificates
layout: post
---

## Introduction
Suppose the domain is `example.com`, associated with a static IP address, for which one had generated a SSL certficate before. We will migrate to a new server, at the end of the process, for the same domain name `example.com`.

## A Plan for Migration
1. In [AWS](https://aws.amazon.com), create a new Virtual Machine (VM) instance and a new Elastic IP (static public IP address). Associate the Elastic IP address to the new VM instance.
2. Dissociate the Elastic IP from the old server, associate the Elastic IP to the new server even when both are running live.
3. Set up the web server configuration on the new server.

## Notes
Let's Encrypt, the Certificate Authority (CA) that issues free SSL certificates to websites
[https://letsencrypt.org](https://letsencrypt.org)

`certbot`, the recommended client by Let's Encrypt that handles communication between the user website and the CA
[https://certbot.eff.org](https://certbot.eff.org)

To retrieve an already issued certificate with `certbot`, the http (port number 80) port must be open,

```sh
sudo certbot certonly --standalone -d <domain>
```

To re-configure `nginx` web server with the new certificate, open the relevant `.conf` file, in the section for https (port number usually 443), change the two lines for private key and full chain certificate files,

```
  server {
    listen       443 ssl http2;
    listen       [::]:443 ssl http2;
    server_name  <server_name>;

    ssl_certificate <full_chain_certificate_file>;
    ssl_certificate_key <private_key_file>;

    ...
  }
```

To force reload `nginx` web server,

```sh
sudo service nginx force-reload
```

or on [Alpine Linux](https://www.alpinelinux.org),

```sh
/etc/init.d/nginx reload
```

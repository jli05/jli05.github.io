---
title: Migrate a web server with SSL certificates
layout: post
---

## Introduction
Suppose the domain is `example.com`, associated with a static IP address, for which one had generated a SSL certficate before. We will migrate to a new server, at the end of the process, for the same domain name `example.com`.

## A Plan for Migration
1. In [AWS](https://aws.amazon.com), create a new Virtual Machine (VM) instance and a new Elastic IP (static public IP address). Associate the Elastic IP address to the new VM instance.
2. Now go to your DNS ([Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System)) registrar, create a new DNS entry: `stage1` with the new Elastic IP address. The new VM is bearing host name `stage1.example.com`.
3. Set up everything on `stage1.example.com`. Get it running. Generate an SSL certificate for `stage1.example.com` as a rehearsal with `certbot`, or refer to [Use acme.sh to install SSL certficates](/2023/08/28/acme-SSL-certificates.html).
4. `stage1.example.com` should be fully functional by now. Leave it running for some time. When ready to complete the migration, go into the AWS Management Console, disassociate the Elastic IP address from the new VM instance, and associate it with the Elastic IP address always configured for `example.com`. The VM now bears host name `example.com` instead of `stage1.example.com`. This can even be done with all VMs live running.
5. On this new VM, retrieve the SSL certificate from the Certificate Authority (CA) for `example.com` with `certbot` or `acme.sh`. Re-configure the web server with the new SSL certificate and force reload it.
6. Revoke the certificate for `stage1.example.com`. Go to the DNS setting, and delete the entry for subdomain `stage1`. Back in AWS, the Elastic IP address created at the beginning of these steps should be free of association now, release it. It is **important** to clean up and release these resources, uncleaned DNS configurations may become a security risk.

## Notes
Let's Encrypt, the Certificate Authority (CA) that issues free SSL certificates to websites
[https://letsencrypt.org](https://letsencrypt.org)

`certbot`, the recommended client by Let's Encrypt that handles communication between the user website and the CA
[https://certbot.eff.org](https://certbot.eff.org)

To retrieve a certificate with `certbot`, the http (port number usually 80) port must be open,

```sh
sudo certbot certonly --standalone -d <server_name>
```

To re-configure `nginx` web server with the new certificate, open `/etc/nginx/nginx.conf`, in the section for https (port number usually 443), change the two lines for private key and full chain certificate files,

```
  server {
    listen       443 ssl http2;
    listen       [::]:443 ssl http2;
    server_name  <server_name>;
    root         /usr/share/nginx/html;

    ssl_certificate <full_chain_certificate_file>;
    ssl_certificate_key <private_key_file>;

    ...
  }
```

To force reload `nginx` web server,

```sh
sudo service nginx force-reload
```

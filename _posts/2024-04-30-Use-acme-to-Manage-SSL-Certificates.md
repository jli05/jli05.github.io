---
title: "Use acme.sh to Manage SSL Certificates"
layout: post
---

As a follow-up to an [earlier post](/2023/08/28/acme-SSL-certificates.html), if using [Unit](https://unit.nginx.org/) as web server, add `--standalone` parameter to `acme.sh` commands. For example,

```sh
sudo su
curl https://get.acme.sh | sh -s email=<email>
/root/.acme.sh/acme.sh --set-default-ca --server letsencrypt
/root/.acme.sh/acme.sh --issue --standalone -d <domain>

/root/.acme.sh/acme.sh --revoke --standalone --domain <domain>
/root/.acme.sh/acme.sh --renew --standalone --domain <domain>
```

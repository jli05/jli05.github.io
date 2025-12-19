---
title: Renew SSL Certificate for Unit Server
layout: post
---

First use acme.sh to [renew](/2024/04/30/Use-acme-to-Manage-SSL-Certificates.html) the certificate. Then as one example,

```sh
cat /root/.acme.sh/tsterm.com_ecc/fullchain.cer /root/.acme.sh/tsterm.com_ecc/tsterm.com.key >bundle2406.pem
curl -X PUT --data-binary @bundle2406.pem --unix-socket /run/control.unit.sock http://localhost/certificates/bundle2406
curl -X PUT --data '{"certificate": "bundle2406"}' --unix-socket /run/control.unit.sock http://localhost/config/listeners/*:443/tls
rm bundle2406.pem
```

No need to restart the `unit` service. The new certificate is configured now. Lastly to clean up the old certificate,

```sh
curl -X DELETE --unix-socket /run/control.unit.sock http://localhost/certificates/bundle2404
``` 

To check the uploaded certificates,

```sh
curl -X GET --unix-socket /run/control.unit.sock http://localhost/certificates/
```

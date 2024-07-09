---
title: Renew SSL Certificate for Unit Server
layout: post
---

First use acme.sh to [renew](/2024/04/30/Use-acme-to-Manage-SSL-Certificates.html) the certificate. Then follow

```sh
cat .../fullchain.cer .../<domain>.key >bundle...pem
curl -X PUT --data-binary @bundle...pem --unix-socket /run/<control.socket> http://localhost/certificates/bundle..
rm bundle...pem
curl -X PUT --data '{"certificate": "bundle.."}' --unix-socket /run/<control.socket> http://localhost/config/listeners/*:443/tls
```

No need to restart the `unit` service. The new certificate is configured now. Lastly to clean up the old certificate,

```sh
curl -X DELETE --unix-socket /run/<control.socket> http://localhost/certificates/<old_bundle>
``` 

To check the uploaded certificates,

```sh
curl -X GET --unix-socket /run/<control.socket> http://localhost/certificates/
```

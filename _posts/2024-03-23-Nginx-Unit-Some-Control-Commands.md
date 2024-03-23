---
title: "Nginx Unit: Some Control Commands"
layout: post
---

```sh
# set configuration
curl -X PUT --data-binary @config.json --unix-socket /run/control.unit.sock http://localhost/config/

# upload SSL bundle
#
# bundle.pem is a concatenation of certificate + ca + private key,
#  from leaf to root, in that order, 
#
#   cat domain.cer ca.cer domain.key >bundle.pem
#
# or if fullchain.cer is available,
#
#   cat fullchain.cer domain.key >bundle.pem
#
curl -X PUT --data-binary @bundle.pem --unix-socket /run/control.unit.sock http://localhost/certificates/bundle

# delete SSL bundle
curl -X DELETE --unix-socket /run/control.unit.sock http://localhost/certificates/bundle

# change one item in the configuration
curl -X PUT -d '"/var/log/unit/access.log"' --unix-socket /run/control.unit.sock http://localhost/config/access_log

# restart the application
curl --unix-socket /run/control.unit.sock http://localhost/control/applications/flask/restart

# list the SSL certificates, configuration and status
curl --unix-socket /run/control.unit.sock http://localhost
```

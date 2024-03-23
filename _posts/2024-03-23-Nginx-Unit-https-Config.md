---
title: "Nginx Unit: https config" 
layout: post
---


As a follow-up to [previous post](/2024/03/23/Nginx-Unit-Web-Server.html), in order to configure the https protocol, first [upload](https://unit.nginx.org/certificates/) the SSL certificate bundle, then add listener on port 443 in `config.json`:

```json
{
    "listeners": {
        "*:80": {
            "pass": "applications/flask"
        },
        "*:443": {
            "pass": "applications/flask",
            "tls": {
                "certificate": "bundle"
            }
        }
    },

    "applications": {
        "flask": {
            "type": "python 3",
            "working_directory": "<path>",
            "path": "<path>",
            "home": "<path>/venv/",
            "module": "<python_module>",
            "callable": "app"
        }
    },

    "access_log": "/var/log/unit/access.log"
}
```

To rewrite http request into https request, change the listener on port 80,

```json
{
    "listeners": {
        "*:80": {
            "pass": "routes/redirect_to_https" 
        },
        "*:443": {
            "pass": "applications/flask",
            "tls": {
                "certificate": "bundle"
            }
        }
    },

    "applications": {
        "flask": {
            "type": "python 3",
            "working_directory": "<path>",
            "path": "<path>",
            "home": "<path>/venv/",
            "module": "<python_module>",
            "callable": "app"
        }
    },

    "access_log": "/var/log/unit/access.log",

    "routes": {
        "redirect_to_https": [ {
            "action": {
                "return": 301,
                "location": "https://$host$request_uri"
            }
        } ]
    }
}
```

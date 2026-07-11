---
title: Nginx Unit Web Server
layout: post
---

[https://unit.nginx.org](https://unit.nginx.org)

Sample `config.json`:

```json
{
    "listeners": {
        "*:80": {
            "pass": "applications/flask"
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

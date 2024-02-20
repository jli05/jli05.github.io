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

### nginx Web Server
We use [nginx](https://nginx.org) web server. We will incrementally make changes and reload to check,

on systemd-based systems,

```sh
nginx -t
sudo service nginx force-reload
```

or on [Alpine Linux](https://www.alpinelinux.org),

```sh
/etc/init.d/nginx checkconfig
/etc/init.d/nginx reload
```

### The Domain Configuration File
First create a default `/etc/nginx/http.d/<domain>.conf`,

```
# This is a default site configuration which will simply return 404, preventing
# chance access to any other virtualhost.

server {
    listen 80 default_server;
	listen [::]:80 default_server;
	server_name <server_name>;

    # Everything is a 404
	location / {
		return 404;
	}

	# You may need this to prevent return 404 recursion.
	location = /404.html {
		internal;
	}
}
```

### Issue or Re-issue SSL Certificate
For best ease of installation, we need be as a superuser. The `letsencrypt` server appears not returning the necessary nonce if `wget` is used to query it, so `curl` must be installed. Whether to issue a new certificate or reissue an existing one,

```sh
sudo su
curl https://get.acme.sh | sh -s email=<email>
/root/.acme.sh/acme.sh --set-default-ca --server letsencrypt
/root/.acme.sh/acme.sh --issue --nginx /etc/nginx/http.d/<domain>.conf -d <domain>
```

As an aside, to revoke or renew a certificate,

```sh
/root/.acme.sh/acme.sh --revoke --domain <domain>
/root/.acme.sh/acme.sh --renew --domain <domain>
```

To upgrade the `acme` client itself,

```sh
/root/.acme.sh/acme.sh upgrade
```

After issuance of the certificate, press `Ctrl+D` to quit the `su` session and return as normal user. **Back up** the certificate files as soon as you can.

### Update Configuration Files
Now go to `/etc/nginx/nginx.conf` make sure these global SSL parameters exist under the `http` section. Below is the [Alpine Linux](https://www.alpinelinux.org/) version,

```
http {
    ....

	# Enables the specified protocols. Default is TLSv1 TLSv1.1 TLSv1.2.
	# TIP: If you're not obligated to support ancient clients, remove TLSv1.1.
	ssl_protocols TLSv1.2 TLSv1.3;

	# Path of the file with Diffie-Hellman parameters for EDH ciphers.
	# TIP: Generate with: `openssl dhparam -out /etc/ssl/nginx/dh2048.pem 2048`
	#ssl_dhparam /etc/ssl/nginx/dh2048.pem;

	# Specifies that our cipher suits should be preferred over client ciphers.
	# Default is 'off'.
	ssl_prefer_server_ciphers on;

	# Enables a shared SSL cache with size that can hold around 8000 sessions.
	# Default is 'none'.
	ssl_session_cache shared:SSL:2m;

	# Specifies a time during which a client may reuse the session parameters.
	# Default is '5m'.
	ssl_session_timeout 1h;

	# Disable TLS session tickets (they are insecure). Default is 'on'.
	ssl_session_tickets off;

    ...
}
```

Now change `/etc/nginx/http.d/<domain>.conf` to

```
server {
	listen 443 ssl;
	listen [::]:443 ssl;
	server_name <server_name>;

	ssl_certificate /root/.acme.sh/<domain>_ecc/fullchain.cer;
	ssl_certificate_key /root/.acme.sh/<domain>_ecc/<domain>.key;

	location / {
		uwsgi_pass <host>:<port>;
		include uwsgi_params;
	}

	location = /404.html {
		internal;
	}
}

server {
	if ($host = <server_name>) {
		return 301 https://$host$request_uri;
	}

	listen 80;
	listen [::]:80;
	server_name <server_name>;
	return 404;

	location = /404.html {
		internal;
	}
}
```

Reload the web server to test.

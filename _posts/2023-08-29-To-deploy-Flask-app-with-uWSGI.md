---
title: To deploy Flask web applications with uWSGI
layout: post
---

The Flask web applications communicates data by the [Web Server Gatway Interface](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface) (WSGI) specification. It cannot be readily understood by the current web servers such as `nginx` at the front end, hence the need for a middle layer that does the translation of communciation protocols.

If we use `uWSGI` for this purpose, to install it,

```sh
sudo yum install gcc python3-devel
pip3 install --user uwsgi
```

`uWSGI` needs be configured for the web application. Below is an example configuration file `uwsgi.ini`.

```ini
[uwsgi]
socket = :7777
master = true
processes = <number_of_processes>
module = <name_of_webapp_module>:app
master-fifo = /home/ec2-user/www/uwsgi.fifo
logto = /home/ec2-user/www/uwsgi.log
stats = /home/ec2-user/www/stats.socket
```

`<number_of_processes>` is the number of concurrent `uWSGI` server processes to handle requests. `<name_of_webapp_module>` is the name of the Python module you have developed the Flask web application under. For example, for this Flask official [tutorial](https://flask.palletsprojects.com/en/2.3.x/tutorial/), the name of webapp module should be `flaskr`. Lastly, for the above configuration, all the runtime files will be placed under the directory `/home/ec2-user/www` and one has to make sure it exists.

To start the web application and the uWSGI server in the background, in the shell, do

```sh
uwsgi --ini uwsgi.ini &
```

The front end web server `nginx` needs to delegate requests for the web application to `uWSGI`. We go to its configuration file `/etc/nginx/nginx.conf`, and modify the relevant `server` section,

to mount the web app on the root path '/', so that the URL looks `http://<host>`,

```
  server {
    ...
    location / {
		uwsgi_pass 127.0.0.1:7777;
		include uwsgi_params;
	}
    ...
  }
```

or to mount the web app on path '/bbb', so that the URL looks `http://<host>/bbb`,

```
  server {
    ...
    location = /bbb {
        return 301 $scheme://$host$uri/$is_args$args;
    }
    location /bbb/ {
        try_files $uri @bbb;
    }
    location @bbb {
		uwsgi_pass 127.0.0.1:7777;
		include uwsgi_params;
    }
    ...
  }
```

Eventually, reload or restart the web server in the shell,

```sh
sudo service nginx force-reload
```

When `uWSGI` is running, we could print text command to the fifo file `/home/ec2-user/www/uwsgi.fifo` in the shell for control, such as,

```sh
# to quit the web app and uwsgi process
echo q >/home/ec2-user/www/uwsgi.fifo

# to restart the web app and uwsgi process
echo r >/home/ec2-user/www/uwsgi.fifo
```

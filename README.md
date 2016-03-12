# flask-server
Ansible & Vagrant setup for a Flask server, meant to run locally or on AWS

This repository is aimed towards automatically provisioning a VM to serve
web requests via Flask. It manages the VM uses `Vagrant` and handles provisioning
through the use of `Ansible`.

### Stack
Vagrant provisions a web server running [`ubuntu/trusty64`](https://atlas.hashicorp.com/ubuntu/boxes/trusty64)
when using a local VM.
When setting up a VM for AWS, it runs [`panchr/aws-trusty64`](https://atlas.hashicorp.com/panchr/boxes/aws-trusty64),
which is just a wrapper around an Ubuntu server.

On the server, it runs the following web stack:
- [`NginX`](https://www.nginx.com/)
- [`uWSGI`](https://uwsgi-docs.readthedocs.org/en/latest/)
- [`Flask`](http://flask.pocoo.org/), running on [`Python 3.5.0`](https://docs.python.org/3/)

The requests are served as follows: `NginX`->`uWSGI`->`Flask`.

This proxying setup is fairly common for Python-based apps, and thus is what we use.

### Directory Setup
The base directory of the application is `/var/www/$NAME/`, where `$NAME` is the
name of the application (configured in `config.yml`; see [configuration](#configuration)).

Then, the application itself is stored in the `$APP` subdirectory, and static files
are served from the `$STATIC_DIR` subdirectory. These options can all be changed
(except for the base path) in `config.yml`.

### Configuration

Configuration is mostly handled through `config.yml`.

The configuration takes the form of key-value pairs, all contained under
various environment settings. Thus, it enables easier switching of configuration
environments (such `production` and `development`).

Configuration options, which is set up in YAML format:
- `env`: environment name
- `name`: name of project (`$NAME`)
- `web`: web configuration
	- `url`: URL to serve to
	- `static`: relative URL to serve static files to
	- `https`: serve with https or not (`yes` or `no`)*
	- `gzip`: compress responses with gzip or not
	- `protocol`: protocol to serve from (`"https://"` or `"http://"`)
	- `cloudflare`: proxy requests through CloudFlare first*
	- `static_error_pages`: serve error pages through NginX or not
- `app`: app configuration
	- `secret_key`: secret key for using secure cookies
	- `py_version`: Python version to run with Flask (defaults to `3.5.0`)
	- `virtualenv`: name of `virtualenv` to create
	- `directory`: dirctory to serve app from (`$APP`)
	- `static_directory`: directory to serve static files from (`$STATIC_DIR`)
- `user`: user configuration
	- `name`: name of user to create
	- `groups`: list of groups user should be a part of
- `server`: specific server configuration
	- `ufw`: enable UFW firewall or not
	- `swap`: amount of memory swap to install

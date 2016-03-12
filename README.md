# flask-server
Ansible & Vagrant setup for a Flask server, meant to run locally or on AWS

This repository is aimed towards automatically provisioning a VM to serve
web requests via Flask. It manages the VM uses `Vagrant` and handles provisioning
through the use of `Ansible`.

## Stack
Vagrant provisions a web server running [`ubuntu/trusty64`](https://atlas.hashicorp.com/ubuntu/boxes/trusty64)
when using a local VM.
When setting up a VM for AWS, it runs [`panchr/aws-trusty64`](https://atlas.hashicorp.com/panchr/boxes/aws-trusty64),
which is just a wrapper around an Ubuntu server.

On the server, it runs the following web stack:
	- [`NginX`](https://www.nginx.com/)
	- [`uWSGI`](https://uwsgi-docs.readthedocs.org/en/latest/)
	- [`Flask`](http://flask.pocoo.org/), running on [`Python 3.5.0`](https://docs.python.org/3/)

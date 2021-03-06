# Installation

Picatrix can be installed via two ways:

1. docker
2. pip inside a virtualenv

Let's explore both methods:

## Docker

The easiest way to install picatrix is via docker. To use it, first
[install docker](https://docs.docker.com/engine/install/).

To run the docker container use:

```shell
$ git clone https://github.com/google/picatrix.git
```

If you are running on a Linux machine you may try to first run the installation
scrip:

```shell
$ sh install.sh
```

*(if you don't have the git client installed you can also download
the [source code using this link](https://github.com/google/picatrix/archive/main.zip))*

By default the /tmp folder on your host will be mapped into a `data` folder
on the docker container. If you want to change that and point to another
folder on your system (**highly encouraged**), edit the file `docker-latest.yml`
and change the path `/tmp` to a folder of your choosing (just remember that the
folder needs to be writable by uid=1000 and/or gid=1000 if you are running a
Linux based host).

For instance if you are running this on a Windows system, then you will
need to change the `/tmp/` to something like `C:\My Folder\Where I store data`.
Also when running on Windows, there is no `sudo` in front of the commands.

You also need to install `docker-compose`, please follow the instructions
[here](https://docs.docker.com/compose/install/) (the version that is often
included in your source repo might be too old to properly setup the container).

After that, just run:

```shell
$ cd picatrix/docker
$ sudo docker-compose up -d
```

That will download the latest build and deploy the picatrix docker container.
To be able to connect to picatrix, open the following URL in a browser
window:
[http://localhost:8899/?token=picatrix](http://localhost:8899/?token=picatrix)`

You should have a fully working copy of Jupyter running, with an
active picatrix library.

### Upgrade Container

To upgrade the container using the latest build, you can run:

```shell
$ sudo docker-compose pull
$ sudo docker-compose up -d
```

You can also manually pull the new image using:

```shell
$ sudo docker pull us-docker.pkg.dev/osdfir-registry/picatrix/picatrix:latest
```

*or if you are using Docker desktop you can find the docker image, click
on the three dots and select pull*

After updating the image the container needs to be recreated

## Virtualenv

To install picatrix using virtualenv you can use the following commands:

```shell
$ python3 -m venv picatrix_env
$ source picatrix_env/bin/activate
$ pip install picatrix
```

And then to start picatrix you can either run:

```shell
$ jupyter notebook
```

If you want to connect from a colab frontend, then you'll need to run these
two commands as well:

```shell
$ pip install jupyter_http_over_ws
$ jupyter serverextension enable --py jupyter_http_over_ws
```

And then to run the notebook:

```shell
$ jupyter notebook \
  --NotebookApp.allow_origin='https://colab.research.google.com' \
  --port=8888 \
  --NotebookApp.port_retries=0
```

After the notebook is started you need to load up picatrix using the code cell:

```python
from picatrix import notebook_init
notebook_init.init()
```

*This also works if you want to connect to the hosted colab runtime. There you
can simply add a cell with `!pip install --upgrade picatrix` and you should
be able to start using picatrix.*

## Confirm Installation

You can confirm the successful installation by creating a new notebook and type in:
```
%picatrixmagics
```

Picatrix library is already imported and initialized.

## Connect To Colab

### Using Docker Container

In order to connect to the docker container from colab, select the arrow
next to the `Connect` button, select `Connect to local runtime` and type
in the URL `http://localhost:8899/?token=picatrix` into the `Backend URL`
field and hit `CONNECT`.

### Using Virtualenv

Look at the window where the jupyter notebook is being run from. It should
have lines similar to this:

```
    To access the notebook, open this file in a browser:
    ...
    Or copy and paste one of these URLs:
        http://....:8899/?token=...
     or http://127.0.0.1:8899/?token=...
```

Copy the URL that is provided there, replace 127.0.0.1 with localhost and enter
the URL into the `Backend URL` as described in the Docker instructions.

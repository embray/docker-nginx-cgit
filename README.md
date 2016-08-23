# docker-nginx-cgit

A Docker image for Cgit running over Nginx.

[![](https://badge.imagelayers.io/emarcs/nginx-cgit:latest.svg)](https://imagelayers.io/?images=emarcs/nginx-cgit:latest 'Get your own badge on imagelayers.io')

This is a docker image for Cgit, a lightweight web interface for git.

## Installation

Download this image from the docker hub:

```shell
docker pull emarcs/nginx-cgit
```

Then wait for docker to do its magic.

## Usage

To launch the container, just the run command like this:

```sh
docker run -d \
           -p 2340:80 \
           --name nginx-git-srv \
           -v /git:/srv/git \
           emarcs/nginx-git
```

In the above example the **/git** folder of the the host
is used inside the **/srv/git** folder of the container,
the HTTP port is mapped on the host on port **2340**.

### Using a custom Nginx configuration

An example using docker-compose of how to mount a custom
configuration for Nginx:

```yml
test_srv:
  image: emarcs/nginx-cgit
  ports:
    - 8181:80
  volumes:
    - /srv/git:/srv/git
    # custom nginx configuration
    - ./default.conf:/etc/nginx/sites-available/default
  environment:
    CGIT_TITLE: 'My awesome git repos'
    CGIT_DESC: 'Presented by Cgit on Docker'
    CGIT_VROOT: '/'
    # check section-from-path in cgit docs
    CGIT_SECTION_FROM_STARTPATH: 0
```

### Cache

cgit provides a cache mechanism (cf https://git.zx2c4.com/cgit/tree/cgitrc.5.txt ), which will
by default store entries in `/var/cache/cgit`.

You can eventually map this folder to your disk:
```sh
docker run -d \
           -p 2340:80 \
           --name nginx-git-srv \
           -v /git:/srv/git \
           -v /mnt/disk/cgit/cache:/var/cache/cgit \
           emarcs/nginx-git
```


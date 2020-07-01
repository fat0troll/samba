# Samba in Docker container

This repository contains sources for [fat0troll/samba](https://hub.docker.com/repository/docker/fat0troll/samba) - Docker container for as simple as possible sharing experience. After tiny tinkering you will get running Samba, ready to be advertized to local network via external mDNS server like Avahi (and to host Time Machine backups for example).
  
**Note 1**: This container requires `host` or `macvlan` network type, and will **not** work in `bridge` (default mode) network. See `examples/docker-compose.yml` for details. If you can/want to make it work in `bridge` mode, PRs are welcome (see Note 3).

**Note 2**: This container is tested under Linux and built for linux/amd64 only. That's because this is the only architecture used in my servers at the time. If you want to, you can send PR with changes for building image for other architectures.

## Preparations

There is no custom scripts or similar, this container consumes standard configuration files from Samba. Example configuration file for Samba included in `examples/samba` directory. 

## Environment variables

There is one set of environment variables this container consumes: `USER*` (where `*` is replaced with a number, but technically it may be everything). The format of variables is `username,group,UID`: you can create inside a container user with your host UID and it will write files to your share locations seamlessly with host. Example of `env` file with some defined users located in `examples` directory.

## Starting from command-line example

```
$ docker run -it --name samba --net=host \
	-v /path/to/samba/configs:/etc/samba \
	-v /path/to/samba/lib:/var/lib/samba \
	-v /path/to/shares:/data \
	fat0troll:samba
```

## Starting from docker stack example

See `examples/docker-compose.yaml`. This file may be deployed by command:

```
$ docker stack deploy -c examples/docker-compose.yaml samba
```
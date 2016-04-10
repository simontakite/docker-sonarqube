# This documentation is baking ...

# About absolootly/docker-sonarqube

- [Introduction](#introduction)
- [Getting started](#getting-started)
  - [Installation](#installation)
  - [Quickstart](#quickstart)
  - [Authentication](#authentication)
- [Maintenance](#maintenance)
  - [Upgrading](#upgrading)
  - [Shell Access](#shell-access)

# Introduction

This is a`Dockerfile`method to create a [Docker](https://www.docker.com/) container image for [Sonarqube](http://sonarqube.io/).

Sonarqube is an open source, BSD licensed, advanced key-value cache and store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.

# Getting started

## Installation

Automated builds of the image are available on [Dockerhub](https://hub.docker.com/r/absolootly/docker-sonarqube) and is the recommended method of installation.

```bash
docker pull absolootly/docker-sonarqube:latest
```

Alternatively you can build the image yourself.

```bash
docker build -t absolootly/docker-sonarqube github.com/simontakite/docker-sonarqube
```

## Quickstart

Start Sonarqube using:

```bash
docker run --name sonarqube -d --restart=always \
  --publish 6379:6379 \
  --volume /srv/docker/sonarqube:/var/lib/sonarqube \
  absolootly/docker-sonarqube:latest
```
*Alternatively, you can use the sample [docker-compose.yml](https://github.com/simontakite/docker-sonarqube/blob/master/docker-compose.yml) file to start the container using [Docker Compose](https://docs.docker.com/compose/)*

## Command-line arguments

You can customize the launch command of Sonarqube server by specifying arguments to `sonarqube-server` on the `docker run` command. For example the following command will enable the Append Only File persistence mode:

```bash
docker run --name sonarqube -d --restart=always \
  --publish 6379:6379 \
  --volume /srv/docker/sonarqube:/var/lib/sonarqube \
  absolootly/docker-sonarqube:latest --appendonly yes
```

Please refer to http://sonarqube.io/topics/config for further details.

## Persistence

For Sonarqube to preserve its state across container shutdown and startup you should mount a volume at `/var/lib/sonarqube`.

> *The [Quickstart](#quickstart) command already mounts a volume for persistence.*

SELinux users should update the security context of the host mountpoint so that it plays nicely with Docker:

```bash
mkdir -p /srv/docker/sonarqube
chcon -Rt svirt_sandbox_file_t /srv/docker/sonarqube
```

## Authentication

To secure your Sonarqube server with a password, specify the password in the `REDIS_PASSWORD` variable while starting the container.

```bash
docker run --name sonarqube -d --restart=always \
  --publish 6379:6379 \
  --env 'REDIS_PASSWORD=sonarqubepassword' \
  --volume /srv/docker/sonarqube:/var/lib/sonarqube \
  absolootly/docker-sonarqube:latest
```

Clients connecting to the Sonarqube server will now have to authenticate themselves with the password `sonarqubepassword`.

Alternatively, the same can also be achieved using the [Command-line arguments](#command-line-arguments) feature to specify the `--requirepass` argument.

## Logs

By default the Sonarqube server logs are sent to the standard output. Using the [Command-line arguments](#command-line-arguments) feature you can configure the Sonarqube server to send the log output to a file using the `--logfile` argument:

```bash
docker run --name sonarqube -d --restart=always \
  --publish 6379:6379 \
  --volume /srv/docker/sonarqube:/var/lib/sonarqube \
  absolootly/docker-sonarqube:latest --logfile /var/log/sonarqube/sonarqube-server.log
```

To access the Sonarqube logs you can use `docker exec`. For example:

```bash
docker exec -it sonarqube tail -f /var/log/sonarqube/sonarqube-server.log
```

# Maintenance

## Upgrading

To upgrade to newer releases:

  1. Download the updated Docker image:

  ```bash
  docker pull absolootly/docker-sonarqube:latest
  ```

  2. Stop the currently running image:

  ```bash
  docker stop sonarqube
  ```

  3. Remove the stopped container

  ```bash
  docker rm -v sonarqube
  ```

  4. Start the updated image

  ```bash
  docker run --name sonarqube -d \
    [OPTIONS] \
    absolootly/docker-sonarqube:latest
  ```

## Shell Access

```bash
docker exec -it sonarqube bash
```

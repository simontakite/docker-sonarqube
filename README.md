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

This is a`Dockerfile`method to create a [Docker](https://www.docker.com/) container image for [Sonarqube](http://sonarqube.org/).

SonarQube software (previously called Sonar) is an open source quality management platform, dedicated to continuously analyze and measure technical quality, from project portfolio to method. If you wish to extend the SonarQube platform with open source plugins, have a look at our plugins forge.

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
  --publish 8800:9000 \
  --volume /srv/docker/sonarqube:/var/lib/sonarqube \
  absolootly/docker-sonarqube:latest
```

## Command-line arguments

You can customize the launch command of Sonarqube server by specifying arguments to `sonarqube-server` on the `docker run` command. For example the following command will enable the Append Only File persistence mode:

```bash
docker run --name sonarqube -d --restart=always \
  --publish 8800:9000 \
  --volume /srv/docker/sonarqube:/var/lib/sonarqube \
  absolootly/docker-sonarqube:latest --appendonly yes
```

Please refer to http://docs.sonarqube.org/display/SONAR/Documentation for further details.

## Authentication

Browse to `localhost:8800` username `admin` password `admin`

## Logs

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
docker exec -it sonarqube /bin/bash
```

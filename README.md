# Redis container image running on Alpine Linux

[![Docker Automated build](https://img.shields.io/docker/automated/yobasystems/alpine-redis.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/yobasystems/alpine-redis/)
[![Docker Pulls](https://img.shields.io/docker/pulls/yobasystems/alpine-redis.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/yobasystems/alpine-redis/)
[![Docker Stars](https://img.shields.io/docker/stars/yobasystems/alpine-redis.svg?style=for-the-badge&logo=docker)](https://hub.docker.com/r/yobasystems/alpine-redis/)

[![Alpine Version](https://img.shields.io/badge/Alpine%20version-v3.20.2-green.svg?style=for-the-badge&logo=alpine-linux)](https://alpinelinux.org/)
[![Redis Version](https://img.shields.io/badge/redis%20version-v7.2.5-green.svg?style=for-the-badge&logo=redis)](https://redis.io/)



This container image [(yobasystems/alpine-redis)](https://hub.docker.com/r/yobasystems/alpine-redis/) is based on the minimal [Alpine Linux](http://alpinelinux.org/) with version 7.2.5 of [Redis](https://redis.io/)

### Alpine Version 3.20.2 (Released 2024-07-22)
##### Redis Version 7.2.5

----

## Table of Contents

- [What is Alpine Linux?](#what-is-alpine-linux)
- [Features](#features)
- [Architectures](#architectures)
- [Tags](#tags)
- [Layers & Sizes](#layers--sizes)
- [How to use this image](#how-to-use-this-image)
- [Image contents & Vulnerability analysis](#image-contents--vulnerability-analysis)
- [Source Repositories](#source-repositories)
- [Container Registries](#container-registries)
- [Links](#links)
- [Donation](#donation)


## 🏔️ What is Alpine Linux?
Alpine Linux is a Linux distribution built around musl libc and BusyBox. The image is only 5 MB in size and has access to a package repository that is much more complete than other BusyBox based images. This makes Alpine Linux a great image base for utilities and even production applications. Read more about Alpine Linux here and you can see how their mantra fits in right at home with container images.

## What is Redis?
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

## ✨ Features

* Minimal size only, minimal layers
* Memory usage is minimal on a simple install.

## 🏗️ Architectures

* ```:amd64```, ```:x86_64``` - 64 bit Intel/AMD (x86_64/amd64)
* ```:arm64v8```, ```:aarch64``` - 64 bit ARM (ARMv8/aarch64)
* ```:arm32v7```, ```:armhf``` - 32 bit ARM (ARMv7/armhf)

#### 📝 PLEASE CHECK TAGS BELOW FOR SUPPORTED ARCHITECTURES, THE ABOVE IS A LIST OF EXPLANATION

## 🏷️ Tags

* ```:latest``` latest branch based (Automatic Architecture Selection)
* ```:master``` master branch usually inline with latest
* ```:amd64```, ```:x86_64```  amd64 based on latest tag but amd64 architecture
* ```:aarch64```, ```:arm64v8``` Armv8 based on latest tag but arm64 architecture
* ```:armhf```, ```:arm32v7``` Armv7 based on latest tag but arm32 architecture

## 📏 Layers & Sizes

![Version](https://img.shields.io/badge/version-amd64-blue.svg?style=for-the-badge)
![MicroBadger Layers (tag)](https://img.shields.io/docker/layers/yobasystems/alpine-redis/amd64.svg?style=for-the-badge)
![MicroBadger Size (tag)](https://img.shields.io/docker/image-size/yobasystems/alpine-redis/amd64.svg?style=for-the-badge)

![Version](https://img.shields.io/badge/version-aarch64-blue.svg?style=for-the-badge)
![MicroBadger Layers (tag)](https://img.shields.io/docker/layers/yobasystems/alpine-redis/aarch64.svg?style=for-the-badge)
![MicroBadger Size (tag)](https://img.shields.io/docker/image-size/yobasystems/alpine-redis/aarch64.svg?style=for-the-badge)

![Version](https://img.shields.io/badge/version-armhf-blue.svg?style=for-the-badge)
![MicroBadger Layers (tag)](https://img.shields.io/docker/layers/yobasystems/alpine-redis/armhf.svg?style=for-the-badge)
![MicroBadger Size (tag)](https://img.shields.io/docker/image-size/yobasystems/alpine-redis/armhf.svg?style=for-the-badge)


## 🚀 How to use this image
## Environment Variables:

## Creating an instance

To use this image include `FROM yobasystems/alpine-redis` at the top of your Dockerfile.

```bash
docker run --name redis -p 6379:6379 yobasystems/alpine-redis
```

To use persistent data , then use the volume var:

```bash
docker run --name redis -p 6379:6379 -v /data/redis:/data yobasystems/alpine-redis
```

## Docker Compose example:

```yalm
version: '2'
services:
redis:
  image: yobasystems/alpine-redis
  expose:
    - "6379"
  volumes:
    - /data/redis:/data
  restart: always
```

## 🔍 Image contents & Vulnerability analysis

| PACKAGE NAME          | PACKAGE VERSION | VULNERABILITIES |
|-----------------------|-----------------|-----------------|


## 📚 Source Repositories

* [Github - yobasystems/alpine-redis](https://github.com/yobasystems/alpine-redis)

* [Gitlab - yobasystems/alpine-redis](https://gitlab.com/yobasystems/alpine-redis)

* [Bitbucket - yobasystems/alpine-redis](https://bitbucket.org/yobasystems/alpine-redis/)


## 🐳 Container Registries

* [Dockerhub - yobasystems/alpine-redis](https://hub.docker.com/r/yobasystems/alpine-redis/)

* [Quay.io - yobasystems/alpine-redis](https://quay.io/repository/yobasystems/alpine-redis)

## 🔗 Links

* [Yoba Systems](https://www.yobasystems.co.uk/)

* [Github - Yoba Systems](https://github.com/yobasystems/)

* [Dockerhub - Yoba Systems](https://hub.docker.com/u/yobasystems/)

* [Quay.io - Yoba Systems](https://quay.io/organization/yobasystems)

* [Maintainer - Dominic Taylor](https://github.com/dominictayloruk)

## 💰 Donation

[![BMAC](https://img.shields.io/badge/BUY%20ME%20A%20COFFEE-£5-blue.svg?style=for-the-badge&logo=buy-me-a-coffee)](https://www.buymeacoffee.com/dominictayloruk?new=1)

[![BITCOIN](https://img.shields.io/badge/BTC-bc1q7hy8qmyvq7rw6slrna7yffcdnj9rcg4e9xjecc-blue.svg?style=for-the-badge&logo=bitcoin)](bitcoin:bc1q7hy8qmyvq7rw6slrna7yffcdnj9rcg4e9xjecc)

[![ETHEREUM](https://img.shields.io/badge/ETH-0xb6bE2e4da3d86b50Bdae1F9B6960c23dd87C532C-blue.svg?style=for-the-badge&logo=ethereum)](ethereum:0xb6bE2e4da3d86b50Bdae1F9B6960c23dd87C532C)
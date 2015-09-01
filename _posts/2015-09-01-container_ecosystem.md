---
layout: post
title: Container ecosystem - applications, tools and technologies.
comments: true
tags:
- docker
- containers
- kubernetes
- mesos
- marathon
- consul
- etcd
- flannel
- weave
- kitematic
- rocket
- coreos
- calico
---
Container ecosystem is evolving and bringing along with it a hell lot of other technologies
and tools in its ecosystem. This post aims to list out all such various tools and technologies
that are in creating platforms using containers.

| Applications, tools and technologies | Description|
|--------------------|------------|
|![docker logo](/assets/docker.png)| [Docker](https://www.docker.com/). The first citizen of container ecosystem. The *de-facto* standard for containers. If it's a container, it's most likely docker. |
|![coreos logo](/assets/coreos.jpeg)| [Coreos](https://coreos.com/). The operating system which supposedly runs docker(containers) the best. |
|![mesos logo](/assets/mesos.png)| [Apache Mesos](http://mesos.apache.org/). The gold standard for data center resource scheduling. |
|![marathon logo](/assets/marathon.jpeg)| [Marathon](https://mesosphere.github.io/marathon/). A cluster-wide init and control system for services in cgroups or Docker containers. Runs on top of [Apache Mesos](http://mesos.apache.org/).|
|![kubernetes logo](/assets/kubernetes.jpeg)| [Kubernetes](http://kubernetes.io/). An orchestration system for Docker containers. Distilled with years of experience from *Google*.|
|![rocket logo](/assets/rocket.jpeg)| [Rocket](https://github.com/coreos/rkt). Container runtime from coreos.|
|![etcd logo](/assets/etcd.jpeg)| [etcd](https://coreos.com/etcd/). A highly-available key value store for shared configuration and service discovery.|
|![consul logo](/assets/consul.jpeg)| [consul](https://www.consul.io/). Tool for discovering and configuring services in your infrastructure. Provides service discovery, health check, key/value store and supports multi datacenter ground up.|
|![flocker logo](/assets/flocker.png)| [Flocker](https://clusterhq.com/flocker/introduction/). Data volume manager for your Dockerized applications. Unlike a Docker data volume which is tied to a single server, a Flocker data volume, called a dataset, is portable and can be used with any container in your cluster.|
|![calico logo](/assets/calico.png)| [Calico](http://www.projectcalico.org/). An open source solution for virtual networking|
|![weave logo](/assets/weave.png)| [Weave](http://weave.works/). Weave creates a virtual network that connects Docker containers deployed across multiple hosts and enables their automatic discovery.|
|![flannel logo](/assets/flannel.png)| [Flannel](https://github.com/coreos/flannel). flannel is a virtual network that gives a subnet to each host for use with container runtimes.|
|**_Coreos Fleet_**| [Fleet](https://coreos.com/using-coreos/clustering/). With fleet, you can treat your CoreOS cluster as if it shared a single init system.|
|![swarm logo](/assets/dockerswarm.svg)| [Docker Swarm](https://www.docker.com/docker-swarm). Provides native clustering capabilities to turn a group of Docker engines into a single, virtual Docker Engine|
|![compose logo](/assets/dockercompose.svg)| [Docker Compose](https://www.docker.com/docker-compose). Docker Compose allows you to define your multi-container application with all of its dependencies in a single file, then spin your application up in a single command.|
|![machine logo](/assets/dockermachine.svg)| [Docker Machine](http://docs.docker.com/machine/). Machine lets you create Docker hosts on your computer, on cloud providers, and inside your own data center.|
|![kitematic logo](/assets/kitematic.png)| [Kitematic](https://kitematic.com/). Kitematic is the fastest and easiest way to start using Docker on your laptop.|
|![packer logo](/assets/packer.png)| Packer is a tool for creating machine and container images for multiple platforms from a single source configuration.|

**p.s**: I've added the tools/technologies which i am aware of. In case you find anything missing in the list,
drop a comment below or dm me on twitter and i would be more than happy to add them.

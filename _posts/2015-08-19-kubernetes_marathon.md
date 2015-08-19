---
layout: post
title: Kuberetes vs Mararthon- how they fare
comments: true
tags:
- kubernetes
- mesos
- marathon
- docker
- containers
---

[Kubernetes(k8s)](http://kubernetes.io/) is trying to evolve itself as the defacto standard for container
management. Backed by google it has all the odds to succeed. [Marathon](https://mesosphere.github.io/marathon/) is another
such application which can be used on top of mesos for container management and has been around for a while. In this post we
will try to see how they fare against each other.

| Feature | Kubernetes(K8s)| Marathon| K8s +'s| Marathon +'s|
|----------|--|--|--|--|
| Ability to run Docker and other container types | no, only docker as on date | yes | -1 | +1 |
| Ability to run non container applications | no, only docker containers | yes. can run non-containerized applications too | -1 | +1 |
| Working with Mesos | yes | yes | +1 | +1 |
| Working with other underlying infra | yes, not bound to mesos. Can work independently and also with other platforms like [OpenStack](https://www.openstack.org/) | no, strongly tied to mesos | +1 | -1 |
| Run containers with automatic recovery of failed containers | yes | yes | +1 | +1 |
| Ability to run co-scheduled, co-located containers | yes | no | +1 | -1 |
| Ability to launch sidekick containers | yes | no | +1 | -1 |
| REST API | yes | yes | +1 | +1 |
| UI | no | yes | -1 | +1 |
| Batteries included rolling updates and upgrades | yes | sort of. was introduced in later versions | +1 | +1 |
| Service discovery, load balancing, inbuilt | yes | no. need to integrate with consul, haproxy | +1 | -1 |

_* as on 19/08/15_

### Running K8s on top of Mesos
With the Mesos integration layer, K8s acts as a Mesos framework, scheduling pods as Mesos tasks. K8s-Mesos integrates the K8s scheduler API with the Mesos scheduler API, matching up requested pods to resource offers. Each scheduled pod is also a Mesos task. From a user’s perspective, the integration is transparent. Mesos is the “gold standard” for large-scale production clusters running containers. This integration would give best container management experience with best in class Mesos resource scheduler.

### So, Kubernetes or Marathon?
If you are **only** into containers, then K8s may be the way to go. Because

* K8s was built ground up with the sole aim of deploying, orchestrating and maintaining containers drawing on over ten years of experience running containers at massive scale @ Google.
* Personally, i feel being backed by google and now collaborating with [Mesosphere](https://mesosphere.com/), it has more muscle power and mileage to outlast marathon.
* The recently formed [Cloud Native Computing Foundation](https://cncf.io/), where both Google and Mesosphere are part of have made K8s as the seed technology along with Mesos. This vindicates the point mentioned above.

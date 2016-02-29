---
layout: post
title: S3 compatible storage systems
comments: true
tags:
- Amazon S3
- S3 api
- object storage
- storage api
---

[Amazon S3](https://aws.amazon.com/s3/) has become the de facto standard for object storage. In one of
our projects we had to evaluate various storage systems compatible with S3 API.
The following is a list of storage systems that have [Amazon S3](https://aws.amazon.com/s3/) api compatible interfaces.

- [Openstack swift](http://docs.openstack.org/developer/swift/)

- [Skylable](http://www.skylable.com/products/sx/)

- [Basho riak](http://docs.basho.com/riakcs/latest/)

- [Red Hat ceph](http://ceph.com/)

- \*[Rackspace cloud files](https://www.rackspace.com/en-us/cloud/files)

- ^[caringo](https://www.caringo.com/solutions/cloud-storage.html)

- [Minio](https://www.minio.io/)

- \*[Google cloud storage](https://cloud.google.com/storage/)

- ^[exoscale](https://www.exoscale.ch/open-cloud/storage/)

- [OpenIO](http://openio.io/product/)

\* - managed, ^ - licensed, rest - open source

While the advantages of using S3 is obvious, others (non-managed ones) have the same lifecycle of hugging hardware + software learning curve + ops
maintenance cycles.

---
layout: post
title: Ring, What’s your balance ?
comments: true
redirect_from: "/2014/08/31/ring_whats_your_balance"
permalink: ring_whats_your_balance
---

![balance](/assets/balance.jpg)

In Swift, the Ring is one of the most important structure and file. It defines where the data will be stored in the cluster. It defines whole of your swift cluster. There are many properties of a ring and one such is balance.

So, whats the balance of a ring ?
From the source
> "The balance value is the highest percentage off the desired amount of partitions a given device
wants. For instance, if the “worst” device wants (based on its weight relative to the sum of all the devices’ weights) 123 partitions and it has 124 partitions, the balance value would be 0.83 (1 extra / 123 wanted * 100 for percentage)."

So, balance is the **maximum percentage deviation** from the desired number of partitions a device wants.

Lets consider an example swift cluster with 3 nodes and 10 disks on each node with equal weight, with partition power of 10 and 3 replicas.

The total number of partitions = 2^10 * 3 = 3072 partitions.

ideally, these 3072 partitions should be equally distributed across 30 nodes which would be 3072/30 =  102.4
So, the desired partition each device wants is **102.4**.

Now, lets inspect the object ring using swift-ring-builder.

Lets examine an example ring. To create and view contents of a ring, swift-ring-builder command is used.

{% highlight bash %}
$ swift-ring-builder object.builder 
object.builder, build version 30
1024 partitions, 3.000000 replicas, 1 regions, 1 zones, 30 devices, 0.59 balance
The minimum number of hours before a partition can be reassigned is 1
Devices:    id  region  zone      ip address  port  replication ip  replication port      name weight partitions balance meta
             0       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb1 100.00        102   -0.39 
             1       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb2 100.00        103    0.59 
             2       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb3 100.00        102   -0.39 
             3       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb4 100.00        102   -0.39 
             4       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb5 100.00        102   -0.39 
             5       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb6 100.00        103    0.59 
             6       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb7 100.00        103    0.59 
             7       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb8 100.00        103    0.59 
             8       1     1        10.0.0.1  6000        10.0.0.1              6000      sdb9 100.00        102   -0.39 
             9       1     1        10.0.0.1  6000        10.0.0.1              6000     sdb10 100.00        102   -0.39 
            10       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb1 100.00        102   -0.39 
            11       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb2 100.00        102   -0.39 
            12       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb3 100.00        103    0.59 
            13       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb4 100.00        103    0.59 
            14       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb5 100.00        102   -0.39 
            15       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb6 100.00        103    0.59 
            16       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb7 100.00        102   -0.39 
            17       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb8 100.00        102   -0.39 
            18       1     1        10.0.0.2  6000        10.0.0.2              6000      sdb9 100.00        102   -0.39 
            19       1     1        10.0.0.2  6000        10.0.0.2              6000     sdb10 100.00        103    0.59 
            20       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb1 100.00        102   -0.39 
            21       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb2 100.00        103    0.59 
            22       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb3 100.00        103    0.59 
            23       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb4 100.00        103    0.59 
            24       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb5 100.00        102   -0.39 
            25       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb6 100.00        103    0.59 
            26       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb7 100.00        102   -0.39 
            27       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb8 100.00        102   -0.39 
            28       1     1        10.0.0.3  6000        10.0.0.3              6000      sdb9 100.00        102   -0.39 
            29       1     1        10.0.0.3  6000        10.0.0.3              6000     sdb10 100.00        102   -0.39 
{% endhighlight %}

Lets consider device with id 0. The number of partitions on it is 102. The desired number of partitions is 102.4. So, it’s ‘off’ by 0.4 on negative side. Hence balance of this device is
0.4/102.4 * 100 = 0.39. This means this device has 0.39% lesser number of partitions than the total desired partitions.

Similarly, the balance is calculated for each of the device in the ring, in this case 30 devices.

The balance of the ring is the highest modulo value of the balance of a device in the list of devices present in the ring. In this case, that value is 0.59 present in devices with id 1, 5, 6, 7 …23, 25.

With this, for balance to be 0(the ideal world), the total number of partitions should be evenly divisible by total number of devices in the ring.

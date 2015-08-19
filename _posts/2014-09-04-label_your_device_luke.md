---
layout: post
title: Label your devices, luke!
comments: true
---

![label](/assets/storage-label.jpg)
A swift ring consists of node ip addresses and the devices(disks) attached to it. Its extremely important to have a track of these devices that need to be mounted. This is because on a system reboot, the devices may come up in different order. e.g. a /dev/sdb on current boot may be presented as /dev/sdc on next reboot. This would lead to chaotic behavior since a ring consists of storage-node:disk tuple and if a disk changes, all the partitions that would land up on the disk are totally different. Hence labeling the disk drive is quintessential for the drive to be presented same way across system reboots.
Labeling is done by specifying the -L option while creating xfs file system.
Now, we will walk through on how to format and label a given disk.
Lets assume the disk you have is /dev/sdb.

1.Partition the whole device as a single partition. One can use either mkfs or parted.
(Note: there are some problems creating file system without partitions)

{% highlight bash %}
#parted -s /dev/sdb mklabel gpt 1
#parted -s /dev/sdb mkpart data xfs 0% 100% 1
{% endhighlight %}

2.Create xfs file system on partition using label
{% highlight bash %}
#mkfs.xfs -f -L d1 /dev/sdb1
{% endhighlight %}

3.Verify the label on disk
{% highlight bash %}
# xfs_admin -lu /dev/sdb1
label = "d1"
UUID = a7b22152-5418-4222-84c0-408018fe1150
{% endhighlight %}
The UUID on your system may vary from this.

4.Mount the created file system
{% highlight bash %}
#mount -t xfs -o noatime,nodiratime,logbufs=8 -L d1 /srv/node/d1
{% endhighlight %}

5.Add the mount commands to a script and [make the script run at boot time](http://www.debian-administration.org/article/28/Making_scripts_run_at_boot_time_with_Debian). Save the following contents to a file – mount_disks.sh.
{% highlight text %}
#mount -t xfs -o noatime,nodiratime,logbufs=8 -L d1 /srv/node/d1
#mount -t xfs -o noatime,nodiratime,logbufs=8 -L d2 /srv/node/d2
#mount -t xfs -o noatime,nodiratime,logbufs=8 -L d3 /srv/node/d3
#mount -t xfs -o noatime,nodiratime,logbufs=8 -L d4 /srv/node/d4
{% endhighlight %}

Run mount_disks.sh during boot time.
Once, this is done, the disk is mounted using label d1 and even across reboots.

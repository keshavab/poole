---
layout: post
title: Swift – Object metadata explained
comments: true
redirect_from: "/2014/09/24/swift_object_metadata"
permalink: swift_object_metadata
---

![metadata](/assets/metadata.jpg)
Openstack Swift – we all know stores its objects reliably, in a replicated manner. How does swift store the information about the object itself ? i.e. metadata information ?

The answer is  xattrs. a.k.a extended attributes.
**Extended file attributes** is a file system feature that enables users to associate computer files with metadata not interpreted by the file system.  These are properties organized in (name, value) pairs, optionally set to files or directories in order to record information which cannot be stored in the file itself.

Names are null-terminated printable character strings which are up to 256 bytes in length, while their associated values can contain up to 64 KB of binary data in case of XFS.
More on extended file attributes [here](http://en.wikipedia.org/wiki/Extended_file_attributes).

The extended attributes can be accessed by commands getfattr and set using setfattr.
Consider the following example of a file obj-2 uploaded using swift.
{% highlight bash %}
# swift -A http://127.0.0.1:8080/auth/v1.0 -U test:tester -K testing upload cont1 obj-2
{% endhighlight %}

Using swift-get-nodes, we can see the partition for the uploaded object is 859 and hash is d6e5c4cbcdfb4b1f5e786d4e624b24e4
{% highlight bash %}
# swift-get-nodes  -a /etc/swift/object.ring.gz AUTH_test/cont1/obj-2

Account  	AUTH_test
Container	cont1
Object   	obj-2


Partition	859
Hash     	d6e5c4cbcdfb4b1f5e786d4e624b24e4
{% endhighlight %}

Using this partition information, one can lookup to the file on the swift node.
{% highlight text %}
|-- 1 -> /mnt/sdb1/1
|   `-- node
|       `-- sdb1
|           |-- objects
|           |   |-- 155
|           |   |   `-- 716
|           |   |       `-- 26ddfcaf7d30709da094d31572a8d716
|           |   |           `-- 1412007292.47349.data
|           |   `-- 859
|           |       `-- 4e4
|           |           `-- d6e5c4cbcdfb4b1f5e786d4e624b24e4
|           |               `-- 1412011351.04987.data
{% endhighlight %}

Now, lets examine the contents of “1412011351.04987.data” using getfattr.
{% highlight bash %}
$ getfattr 1412011351.04987.data 
# file: 1412011351.04987.data
user.swift.metadata

$ getfattr -n user.swift.metadata 1412011351.04987.data 
# file: 1412011351.04987.data
user.swift.metadata="�}q(UContent-LengthqU13UnameqU/AUTH_test/cont1/obj-2qUX-Object-Meta-MtimeU1412011318.077730qUETagqU 8ddd8be4b179a529afa5f2ffae4b9858qU
                                                                                                                                                                       X-TimestampU1412011351.04987U
                      Content-Typeq	Uapplication/octet-streamq12u."
{% endhighlight %}

As you can see, swift stores the metadata using extended attributes with the key “user.swift.metadata” and a dictionary as the value.
Lets now uncover the binary value. Following is a python utility program which reads from stdin and unpickles the data. save it as swift-meta.py
{% highlight python %}
# swift-meta.py
import pickle

with open("/dev/stdin") as f:
    print pickle.loads(f.read())
{% endhighlight %}

Now, lets rerun the getfattr on the object with swift-meta.py.(note –only-values, else unpickling will creak)
{% highlight bash %}
$ getfattr -d --only-values -n user.swift.metadata 1412011351.04987.data | python swift-meta.py 
{'Content-Length': '13', 'name': '/AUTH_test/cont1/obj-2', 'Content-Type': 'application/octet-stream', 'ETag': '8ddd8be4b179a529afa5f2ffae4b9858', 'X-Timestamp': '1412011351.04987', 'X-Object-Meta-Mtime': '1412011318.077730'}
{% endhighlight %}

This is what we get when we do a stat on an object. As you can see, the same values stored above are stat’ed.
{% highlight bash %}
$ swift -A http://127.0.0.1:8080/auth/v1.0 -U test:tester -K testing stat cont1 obj-2
       Account: AUTH_test
     Container: cont1
        Object: obj-2
  Content Type: application/octet-stream
Content Length: 13
 Last Modified: Mon, 29 Sep 2014 17:22:32 GMT
          ETag: 8ddd8be4b179a529afa5f2ffae4b9858
    Meta Mtime: 1412011318.077730
 Accept-Ranges: bytes
    Connection: keep-alive
   X-Timestamp: 1412011351.04987
    X-Trans-Id: tx5c513a3673594d6d9ff16-00542998d9
{% endhighlight %}

Now, lets add a custom metadata to see what gets stored in xattrs (–meta my_meta_1:orange).
{% highlight bash %}
$ swift -A http://127.0.0.1:8080/auth/v1.0 -U test:tester -K testing post cont1 obj-2 --meta my_meta_1:orange

$ getfattr -d --only-values -n user.swift.metadata 1412012477.02665.data | python swift-meta.py 
{'Content-Length': '13', 'name': '/AUTH_test/cont1/obj-2', 'ETag': '8ddd8be4b179a529afa5f2ffae4b9858', 'X-Object-Meta-My-Meta-1': 'orange', 'X-Timestamp': '1412012477.02665', 'Content-Type': 'application/octet-stream'}
{% endhighlight %}

As you can see, the user data gets stored as “Object-Meta-My-Meta-1” with value “orange”, along with other set of metadata defined by swift for each object.

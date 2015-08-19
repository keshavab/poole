---
layout: post
title: "Benchmarking nodejs : comparing nodejs and tomcat"
comments: true
---

I've just started exploring nodejs after reading much gung-ho on net about it's scalability and performance.
So, i decided to write a simple program to return a random number, one using node and one using our old friend - java servlet, just to compare.

I am using apache bench (ab) to benchmark these servers.
a. single request
b. 10000 requests ( 5000 concurrent )

Both of them perform the same operation.
- parse the get parameter and generate a random number.
- have some latency ( in real world for a user operation ~1s )
- return the random number.

Below are the programs.

1.Node code
{% highlight js %}
var http = require("http");
var url = require("url");
http.createServer(function(request, response) {
 
     response.writeHead(200, {"Content-Type": "text/plain"});
     var numInput = new Number((require('url').parse(request.url, true).query).number);
     var numOutput = new Number(Math.random() * numInput).toFixed(0);
     // in real world, for a user operation ~1s
     setTimeout(
     function(){
     response.write(numOutput);
     response.end();}
     ,
     1000);     
}).listen(4444);
console.log("server starting...");
{% endhighlight %}

2.Java code
{% highlight java %}
package org.kb.random;
 
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
@WebServlet("/RandomServlet")
public class RandomServlet extends HttpServlet {
 public RandomServlet() {
 }
 
 protected void doGet(HttpServletRequest request,
   HttpServletResponse response) throws ServletException, IOException {
  response.setContentType("text/plain");
  PrintWriter out = response.getWriter();
  try {
   //real world, for a user operation ~1s
   Thread.sleep(1000);
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  out.print(Math.round(Math.random()
    * Integer.valueOf(request.getParameter("number"))));
 }
}
{% endhighlight %}

3.web.xml
{% highlight xml %}
<!--?xml version="1.0" encoding="UTF-8"?-->
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemalocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>random</display-name>
   <servlet>
  <servlet-name>WelcomeServlet</servlet-name>
  <servlet-class>org.kb.random.RandomServlet</servlet-class>
 </servlet>
 <servlet-mapping>
  <servlet-name>WelcomeServlet</servlet-name>
  <url-pattern>/*</url-pattern>
 </servlet-mapping>
</web-app>
{% endhighlight %}

Node version used : v0.6.5 Tomcat version : 7.0.22
Node server running on 4444 , Tomcat one running on 5555
Below are results obtained using ab.

1.Node single request
{% highlight bash %}
C:\Program Files (x86)\Apache Software Foundation\Apache2.2\bin>ab  http://localhost:4444/?number=50
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
 
Benchmarking localhost (be patient).....done
 
 
Server Software:
Server Hostname:        localhost
Server Port:            4444
 
Document Path:          /?number=50
Document Length:        2 bytes
 
Concurrency Level:      1
Time taken for tests:   1.014 seconds
Complete requests:      1
Failed requests:        0
Write errors:           0
Total transferred:      66 bytes
HTML transferred:       2 bytes
Requests per second:    0.99 [#/sec] (mean)
Time per request:       1014.102 [ms] (mean)
Time per request:       1014.102 [ms] (mean, across all concurrent requests)
Transfer rate:          0.06 [Kbytes/sec] received
 
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       0
Processing:  1012 1012   0.0   1012    1012
Waiting:     1011 1011   0.0   1011    1011
Total:       1012 1012   0.0   1012    1012
{% endhighlight %}

2.Node 10000 requests ( 5000 concurrent)
{% highlight bash %}
C:\Program Files (x86)\Apache Software Foundation\Apache2.2\bin>ab -n 10000 -c 5000 http://localhost:4444/?number=50
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
 
Benchmarking localhost (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests
 
 
Server Software:
Server Hostname:        localhost
Server Port:            4444
 
Document Path:          /?number=50
Document Length:        1 bytes
 
Concurrency Level:      5000
Time taken for tests:   6.956 seconds
Complete requests:      10000
Failed requests:        8126
   (Connect: 0, Receive: 0, Length: 8126, Exceptions: 0)
Write errors:           0
Total transferred:      658126 bytes
HTML transferred:       18126 bytes
Requests per second:    1437.55 [#/sec] (mean)
Time per request:       3478.140 [ms] (mean)
Time per request:       0.696 [ms] (mean, across all concurrent requests)
Transfer rate:          92.39 [Kbytes/sec] received
 
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0  12.3      0     506
Processing:  1005 2329 797.1   2408    3777
Waiting:     1002 1839 711.9   1571    3565
Total:       1005 2329 797.3   2409    3778
 
Percentage of the requests served within a certain time (ms)
  50%   2409
  66%   2876
  75%   2944
  80%   3138
  90%   3351
  95%   3708
  98%   3751
  99%   3764
 100%   3778 (longest request)
{% endhighlight %}

3.Servlet - single instance
{% highlight bash %}
C:\Program Files (x86)\Apache Software Foundation\Apache2.2\bin>ab  http://localhost:5555/random/?number=50
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
 
Benchmarking localhost (be patient).....done
 
 
Server Software:        Apache-Coyote/1.1
Server Hostname:        localhost
Server Port:            5555
 
Document Path:          /random/?number=50
Document Length:        2 bytes
 
Concurrency Level:      1
Time taken for tests:   1.002 seconds
Complete requests:      1
Failed requests:        0
Write errors:           0
Total transferred:      168 bytes
HTML transferred:       2 bytes
Requests per second:    1.00 [#/sec] (mean)
Time per request:       1002.100 [ms] (mean)
Time per request:       1002.100 [ms] (mean, across all concurrent requests)
Transfer rate:          0.16 [Kbytes/sec] received
 
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       0
Processing:  1001 1001   0.0   1001    1001
Waiting:     1001 1001   0.0   1001    1001
Total:       1001 1001   0.0   1001    1001
{% endhighlight %}

4.Servlet 10000 requests ( 5000 concurrent)
{% highlight bash %}
C:\Program Files (x86)\Apache Software Foundation\Apache2.2\bin>ab -n 10000 -c 5000  http://localhost:5555/random/?number=50
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/
 
Benchmarking localhost (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests
 
 
Server Software:        Apache-Coyote/1.1
Server Hostname:        localhost
Server Port:            5555
 
Document Path:          /random/?number=50
Document Length:        2 bytes
 
Concurrency Level:      5000
Time taken for tests:   50.191 seconds
Complete requests:      10000
Failed requests:        1911
   (Connect: 0, Receive: 0, Length: 1911, Exceptions: 0)
Write errors:           0
Total transferred:      1678089 bytes
HTML transferred:       18089 bytes
Requests per second:    199.24 [#/sec] (mean)
Time per request:       25095.390 [ms] (mean)
Time per request:       5.019 [ms] (mean, across all concurrent requests)
Transfer rate:          32.65 [Kbytes/sec] received
 
Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   7.1      0     502
Processing:  1112 18386 9622.6  18964   46061
Waiting:     1056 18353 9634.4  18955   46061
Total:       1113 18387 9622.4  18964   46062
 
Percentage of the requests served within a certain time (ms)
  50%  18964
  66%  22128
  75%  24075
  80%  24998
  90%  30986
  95%  36944
  98%  40119
  99%  42091
 100%  46062 (longest request)
{% endhighlight %}

If you can see the **Time per request** lines , the time taken for each request may not be significant for one request, but as number of concurrent requests increase, the performance using node will increase by a huge factor.

Awesome Nodejs !!!
will start exploring more and ramble more !

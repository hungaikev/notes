# Profiling JVM applications

# Profiling JVM applications

bq. Is there any guidance over the memory/cpu usage to tune our services?

I would recommend using a profiler to understand where your application is spending the most time and then adjust accordingly.
In general CPU related profiling is quite hard and it is easy to misread profiling results or miss out on aspects of what the profiler can and cannot do in different modes, so it is important to always verify findings and even more so any optimizations that come out of profiling. If it is about understanding the general performance of your app and potential bottlenecks I think YourKit is pretty good, I also find the Java Flight Recorder (JFR)/Java Mission Control which is shipped with the JDK useful, the flight recorder is relatively low overhead, so can be a good option when you want to run something for quite some while and try to see if there are any events logged that could be hints. Even jvisualvm can be useful though more limited.

For something a bit lower level I'd recommend looking into the sbt-jmh plugin and especially using JFR to generate flame graphs to understand what is going on: "[https://github.com/ktoso/sbt-jmh#automatically-generating-flame-grapghs](https://github.com/ktoso/sbt-jmh#automatically-generating-flame-grapghs)":[https://github.com/ktoso/sbt-jmh#automatically-generating-flame-grapghs](https://github.com/ktoso/sbt-jmh#automatically-generating-flame-grapghs)

For profiling memory, then both YourKit and JFR have allocation recording so you can see what classes are getting allocated over time, this together with taking heap dumps of the running app and analyzing those offline should give you a good hint about what memory is used for.

A few questions you might want to answer during the analyis:

- What is your goal while doing the performance analysis? Increase throughput? Decrease latency of requests?
- Are you confident that your application is CPU bound?

bq. Is there a best practice to configure the port randomly so that we can have multiple docker instances running on the same VM?

I don't think there is any best practice if only looking at Akka. I'd probably prefer pre-defined ports.

One last recommendationif you havent already is to install "Lightbend Telemetry":[https://developer.lightbend.com/docs/cinnamon/latest/home.html](https://developer.lightbend.com/docs/cinnamon/latest/home.html) which provides metrics about actor and cluster related information such as mailbox size, time in mailbox, message processing time, stash size, unhandled messages, deadletters, to name a few. See "[https://developer.lightbend.com/docs/cinnamon/latest/introduction/overview/features.html#akka-related-information](https://developer.lightbend.com/docs/cinnamon/latest/introduction/overview/features.html#akka-related-information)":[https://developer.lightbend.com/docs/cinnamon/latest/introduction/overview/features.html#akka-related-information](https://developer.lightbend.com/docs/cinnamon/latest/introduction/overview/features.html#akka-related-information) for more information about monitoring.

# General performance tuning advice

Run experiments to tune your application. It’s not reliable to rely on assumptions. To tune your application you need to be systematic. Make hypotheses, control variables and test, test, test! It can be a good idea to keep a log of what you tried and what the result was so you can refer to it later.

You can get more information about what’s happening inside your application by using a profiler. At Lightbend we usually use [YourKit](https://www.yourkit.com/) for profiling. YourKit is a commercial product. A free option that works pretty well is [VisualVM](http://visualvm.java.net/).

Profile your app and look at method timings and counts.

- Look for slow methods. Some methods take longer to run than you expect because they use inefficient code. It can be harder to spot slow code in reactive/asynchronous applications, because program logic is often split into lots of little callback functions. However, it is still worth looking for!
- Look for methods that run more times than you expect. For example, if you tested your application with a single request, you’d expect a lot of methods to only run once. If a method runs 100 times then you might have a bug.

Profile and look for parts of your code that wait on locks. Here’s an example of profiling lock contention with VisualVM and YourKit: [http://blogs.mulesoft.org/chasing-the-bottleneck-true-story-about-fighting-thread-contention-in-your-code/](http://blogs.mulesoft.org/chasing-the-bottleneck-true-story-about-fighting-thread-contention-in-your-code/)

# General JVM performance tuning advice

Memory problems can cause performance problems. Allocating and cleaning up objects takes time. If your app creates lots of objects this can lead to a lot of garbage collection overhead. Worse, if your app leaks memory, it can be left with so little usable memory that that it will spend all its time collecting garbage. This can appear to be a performance problem—e.g. you might see connection timeouts—but it’s really a memory leak issue.

You can get a quick idea of your application’s garbage collection behavior by setting some JVM options. The basic one you should set is *-verbose:gc* but you can set other options to get more detail. See here: [https://blog.codecentric.de/en/2014/01/useful-jvm-flags-part-8-gc-logging/](https://blog.codecentric.de/en/2014/01/useful-jvm-flags-part-8-gc-logging/). If you see full garbage collections happening more than once every few minutes then you’ve probably got a problem.

If you suspect a memory problem then you can use a profiler to investigate. Again, [YourKit](https://www.yourkit.com/) or [VisualVM](http://visualvm.java.net/) can will help you. These apps allow you to take a snapshot of an application’s heap and then view it. You can look for object counts that look too high, object trees that use too much memory, etc.

If you can’t connect YourKit or VisualVM to your application to get a heap snapshot then you can use the [jmap](http://docs.oracle.com/javase/7/docs/technotes/tools/share/jmap.html) command line tool. Run *jmap -dump:myapp.dump <pid>*.

If your app sometimes runs out of memory then you can capture a heap dump just before the JVM crashes by setting a command line option: *-XX:+HeapDumpOnOutOfMemoryError*. This setting is usually worth enabling in production. When an application crashes because it runs out of memory, it’s helpful to be able to debug the crash by looking at the heap dump.

You can look for leaks by taking two snapshots at different times and looking for differences. E.g. one snapshot after the app has been running for a while, then another snapshot after another 30s. To make it easier to compare you should try and take a snapshot while your application isn’t doing anything.

Ideally the memory usage of your application should be pretty stable. If you notice that your app has more and more objects allocated and never cleaned up then your application is leaking memory.

Once you’ve made sure you don’t have a memory leak, you can improve performance by reducing the time spent on garbage collection. Even if your application isn’t leaking memory, it can still run slowly if the garbage collector is struggling to keep up with the objects that the application is creating.

One way to speed up garbage collection is to produce less garbage. You get less garbage if you make fewer objects. You can do this by caching objects or by optimizing bits of your code to avoid making objects. Idiomatic functional code can produce lots of objects. If you can’t avoid making objects in your functional code then you might need to write some parts of your program in a more efficient imperative style. (Try to avoid this though!)

Another way to speed things up is to tune the memory and garbage collector. There are lots of different memory and garbage collection settings you can try in modern JVMs.

- Experiment with different memory settings. Play doesn’t actually need that much memory—it can even run with a 32MB heap. Don’t assume that more heap is better. More heap has downsides, such as bigger GC pauses. If you are running with a small heap, run with the 32 bit JVM, as it will save you the GC overhead. Also consider using the CMS garbage collector.
- Setting the Java heap above the machine’s available RAM can lead to swapping. You generally should keep the Java heap size below the amount of physical RAM that’s available.
- Experiment with different GC settings. See: [http://www.slideshare.net/PierreLaporte/pimp-my-gc](http://www.slideshare.net/PierreLaporte/pimp-my-gc)

# General distributed system performance tuning advice

Check for performance problems with connected systems. E.g. problems with a proxy in front of Play or with a database behind it. Run tests to isolate the issues. Try testing by connecting directly to each system rather than through multiple systems. If systems are slow then you can try to tune them. You can use caching to reduce load on other systems.

Check for network performance problems.

Remember more connections to another system doesn’t necessarily make things faster if that system has a fundamental bottleneck. Too many connections could actually slow things down by slicing the workload into inefficiently small pieces.

A specific example of this is database connections. There’s no point having more connections than your database can handle. The database’s limits are based on its disk and CPU. The formula you should probably use is *DB server disk count + (DB server CPU count × 2)*.

# General HTTP performance tuning advice

There are plenty of reasons why a page could load slowly.

Test with different HTTP clients (browsers, *curl*, etc). If necessary you can type HTTP manually using *telnet* for HTTP or *openssl s_client* for HTTPS: [http://blog.yimingliu.com/2008/02/04/testing-https-with-openssl/](http://blog.yimingliu.com/2008/02/04/testing-https-with-openssl/).

Test on different networks and from different devices. You can simulate slow web connections using a debugging HTTP proxy like [Charles](http://www.charlesproxy.com/): [http://roderick.dk/2012/05/11/simulate-slow-web-connections/](http://roderick.dk/2012/05/11/simulate-slow-web-connections/).

Look at the page with browser dev tools and identify which requests are slow. [https://developer.chrome.com/devtools/docs/network](https://developer.chrome.com/devtools/docs/network)

Look for client-side problems, like slow JavaScript, inefficient resource loading, etc. [https://developer.chrome.com/devtools/docs/timeline](https://developer.chrome.com/devtools/docs/timeline)

Try clearing the browser cache. Maybe you’re running an old version of the page!

# Play specific advice

General information on the Play thread model: [https://lightbend.com/blog/why-is-play-framework-so-fast](https://lightbend.com/blog/why-is-play-framework-so-fast)

Don’t switch around between thread pools. Switching threads usually slows down your app. By default, Play will run your applications in a default fork-join pool. You should use this thread pool unless you have a reason not to.

A common mistake is to use the global Scala ExecutionContext. Don’t do this. Use the Play default user ExecutionContext instead. When your action runs, it will start in the Play default user ExecutionContext. If you stick with this thread pool then your code will generally run faster. If you use to the Scala global ExecutionContext then you will incur the overhead of switching threads away from the Play thread pool.

Consider running blocking operations in another thread pool. This seems to go against the previous advice. The rationale is this:

The default thread pools are optimised for lots of short running asynchronous code. They don’t actually have many threads because that would slow down this kind of code. Instead they share the threads and each bit of code runs really quickly. If you block in these threads then it will entirely block one of the few precious threads. You might be left with very few threads available, or even no threads. This slows down all your application.

If you run code that is blocking/synchronous (e.g. database code) then you can try using a different thread pool for that kind of code. You should size this thread pool roughly so most of your threads are blocking and only a few threads are actually running at a time.

You could try using asynchronous logging. Logback’s default logger synchronizes all writes and this can become a big bottleneck. Try removing some of the logging fields. Logging the class name seems to slow things down. More info: [http://blog.takipi.com/how-to-instantly-improve-your-java-logging-with-7-logback-tweaks/](http://blog.takipi.com/how-to-instantly-improve-your-java-logging-with-7-logback-tweaks/)

# Akka Http tuning

Hi Felix,

## What are your recommendations for the client configuration settings.

Because different applicatios have different needs, we do not have a one size fits all recommendation settings for client configuration settings. What I would suggest is to enable DEBUG logging and you will see information about how big the size of the current queue is . You could then start to increase the ``max-open-requests`` number a bit and see how queue size develops over time. Another recommendation is to install Lightbend Telemetry which provides metrics about client requests rates and timings. See [http://developer.lightbend.com/docs/monitoring/latest/instrumentations/akka-http/akka-http.html#akka-http-client-metrics](https://developer.lightbend.com/docs/monitoring/latest/instrumentations/akka-http/akka-http.html#akka-http-client-metrics) for more information about monitoring.

I would also recommend to:

- Check that all response entities are consumed. I mention that because unconsumed response entities are the most frequent bottleneck to pool throughput.
- Check the throughput numbers. What's the average rate of incoming requests? How long does it take for one request to be handled? Check if the resulting rates match up and at which level of parallelism (number of `max-connections`).

If the backend has spare resources, it might indeed make sense to increase the number of `max-connections` to increase parallelism and thus throughput.

Let me know if that helps

ThanksHungai Amuhinda
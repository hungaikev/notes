# Performance Akka

-J-Xmx2G -J-Xms2G -J-server 

-J-XX:+UseNUMA 

-J-XX:+UseG1GC 

-J-XX:+AlwaysPreTouch 

-J-XX:+PerfDisableSharedMem 

-J-XX:+ParallelRefProcEnabled

[https://stackoverflow.com/questions/41297334/akka-http-performance-tuning](https://stackoverflow.com/questions/41297334/akka-http-performance-tuning) 

[https://dzone.com/articles/tuning-an-akka-application](https://dzone.com/articles/tuning-an-akka-application) 

[https://hanskoff.github.io/bootzooka-akka-http-vs-scalatra.html](https://hanskoff.github.io/bootzooka-akka-http-vs-scalatra.html) 

[https://www.gregbeech.com/2018/04/08/akka-http-client-pooling-and-parallelism/](https://www.gregbeech.com/2018/04/08/akka-http-client-pooling-and-parallelism/) 

Hi Thomas,

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

I hope this helps!

Thanks
Hungai Amuhinda

[Profiling JVM applications](./Profiling-JVM-applications-6a6f79d5-6c56-46df-a109-fd5cae600ac8.md)
# FAQ's

# Which Kubernetes Ingress Controller to use with your Lagom Services?

I haven't seen anything that would be Lagom specific for ingress choices, meaning I don't think we have a recommendation here. So the question for your ingress strategy is really about choosing the right way to manage traffic from your external load balancer to your Lagom services. We used nginx in the example just as an example, not a requirement certainly. 

Fortunately, there are many types of Ingress controllers, from: [Traefik](https://traefik.io/), [Voyager](https://github.com/appscode/voyager), [HAProxy](https://github.com/jcmoraisjr/haproxy-ingress), [Nginx](https://github.com/kubernetes/ingress-nginx), [Contour](https://github.com/heptio/contour), [Istio](https://istio.io/docs/tasks/traffic-management/ingress.html), and more. At the end of the day, your choice for an ingress controller should depend on your specific requirements, and a specific implementation's ability to meet your requirements. Different ingress controllers will have different functionality. For example the difference between nginx and traefik comes down to: popularity, maturity and configuration model. `Nginx` is much more popular, much more mature, has a complex configuration model, and requires explicit changes when something in the cluster changes. `Traefik` is less mature, less well known, and can infer configuration changes directly from cluster metadata. 

If you're not familiar with either of the ingress controllers mentioned, I would suggest using `Nginx`. I found a very interesting article of setting up [Nginx ingress controller on-premise](https://medium.com/@maniankara/kubernetes-tcp-load-balancer-service-on-premise-non-cloud-f85c9fd8f43c) 

# Why should use I Lightbend Orchestration.

Lightbend Orchestration already has a number of customers running in production. One thing to keep in mind, the orchestration tools were built on already battle-proven technologies used widely by the community. ie [sbt-native-packager](https://github.com/sbt/sbt-native-packager) for packing apps to Docker containers and [akka-management](https://github.com/akka/akka-management) for  [Bootstraping the cluster](https://developer.lightbend.com/docs/akka-management/current/bootstrap.html) and [Service Discovery](https://developer.lightbend.com/docs/akka-management/current/discovery.html)

# Debugging akka-http client

Because different applicatios have different needs, we do not have a one size fits all recommendation settings for client configuration settings. What I would suggest is to enable DEBUG logging  and you will see information about how big the size of the current queue is . You could then start to increase the @`max-open-requests`@ number a bit and see how queue size develops over time. Another recommendation is to install Lightbend Telemetry which provides metrics about client requests rates and timings. See "http://developer.lightbend.com/docs/monitoring/latest/instrumentations/akka-http/akka-http.html#akka-http-client-metrics":https://developer.lightbend.com/docs/monitoring/latest/instrumentations/akka-http/akka-http.html#akka-http-client-metrics for more information about monitoring.

I would also recommend to:
 * Check that all response entities are consumed. I mention that because unconsumed response entities are the most frequent bottleneck to pool throughput.
 * Check the throughput numbers. What's the average rate of incoming requests? How long does it take for one request to be handled? Check if the resulting rates match up and at which level of parallelism (number of @max-connections@).

If the backend has spare resources, it might indeed make sense to increase the number of @max-connections@ to increase parallelism and thus throughput.

One thing I would suggest though is to not enable pipelining, it will have unexpected effects in most cases.
(i.e. do not change the value of @ pipelining-limit@, leave it at 1, which is the default).

Note that changing this value is also rather discouraged in its documentation:

bc.     # Before increasing this value, make sure you understand the effects of head-of-line blocking.
    # Using a connection pool, a request may be issued on a connection where a previous
    # long-running request hasn't finished yet. The response to the pipelined requests may then be stuck
    # behind the response of the long-running previous requests on the server. This may introduce an
    # unwanted "coupling" of run time between otherwise unrelated requests.
    #
    # See http://tools.ietf.org/html/rfc7230#section-6.3.2 for more info.)

# Memory/CPU usage for akka cluster

Hi Thomas, 

bq. Is there any guidance over the memory/cpu usage to tune our services?

There is no general answer as it all depends on what the application is doing. I would recommend using a profiler to understand where your application is spending the most time and then adjust accordingly.
In general CPU related profiling is quite hard and it is easy to misread profiling results or miss out on aspects of what the profiler can and cannot do in different modes, so it is important to always verify findings and even more so any optimizations that come out of profiling. If it is about understanding the general performance of your app and potential bottlenecks I think YourKit is pretty good, I also find the Java Flight Recorder (JFR)/Java Mission Control which is shipped with the JDK useful, the flight recorder is relatively low overhead, so can be a good option when you want to run something for quite some while and try to see if there are any events logged that could be hints. Even jvisualvm can be useful though more limited.

For something a bit lower level I'd recommend looking into the sbt-jmh plugin and especially using JFR to generate flame graphs to understand what is going on: "https://github.com/ktoso/sbt-jmh#automatically-generating-flame-grapghs":https://github.com/ktoso/sbt-jmh#automatically-generating-flame-grapghs

For profiling memory, then both YourKit and JFR have allocation recording so you can see what classes are getting allocated over time, this together with taking heap dumps of the running app and analyzing those offline should give you a good hint about what memory is used for.

A few questions you might want to answer during the analyis:
- What is your goal while doing the performance analysis? Increase throughput? Decrease latency of requests?
- Are you confident that your application is CPU bound? 

bq. Is there a best practice to configure the port randomly so that we can have multiple docker instances running on the same VM?

I don't think there is any best practice if only looking at Akka. I'd probably prefer pre-defined ports.

One last recommendation, if you haven't already, is to install "Lightbend Telemetry":https://developer.lightbend.com/docs/cinnamon/latest/home.html which provides metrics about actor and cluster related information such as mailbox size, time in mailbox, message processing time, stash size, unhandled messages, deadletters, to name a few. See "https://developer.lightbend.com/docs/cinnamon/latest/introduction/overview/features.html#akka-related-information":https://developer.lightbend.com/docs/cinnamon/latest/introduction/overview/features.html#akka-related-information for more information about monitoring.

I hope this helps!

# What should I choose? AKKA-HTTP Low Level vs Routing DSL

What you loose is not so much about configuration as the ability to re-use pre built building blocks for most if not all common use cases when building HTTP APIs. With the low level API you will have to figure out how to select the right logic to execute for a request (the routing part) and potentially also how to do higher level HTTP logic, for example form parsing or multi part uploads, parsing query parameters and responding with a failure when a required parameter is missing etc.

What you win is potentially some performance, but note that unless your HTTP service is very simple you may very well ending up recreating something like the routing API and have the same or worse overhead (but now you have to maintain that mini-framework yourself).

Let us know if this helps!

# Creating API Documentation for your Akka-Http Service

[https://github.com/akka/akka-http/issues/201](https://github.com/akka/akka-http/issues/201)

[https://github.com/swagger-akka-http/swagger-akka-http](https://github.com/swagger-akka-http/swagger-akka-http)

[https://github.com/jtownson/swakka](https://github.com/jtownson/swakka)

[https://www.twilio.com/blog/2018/03/twilio-guardrail-type-safe-principled.html](https://www.twilio.com/blog/2018/03/twilio-guardrail-type-safe-principled.html)

We (currently) do not maintain a swagger integration as part of the lightbend offerings, however there is one which may be of interest of you,
though it depends which workflow and language you are using.

If you are using Scala and (Maven or sbt) then the recently open sourced https://github.com/twilio/guardrail 
by Twilio (blog here https://www.twilio.com/blog/2018/03/twilio-guardrail-type-safe-principled.html ) could be of interest to you. 

It takes the approach of modeling the APIs using the familiar Swagger file syntax to then generate stubs for the service implementations to fill in.
The nice thing about that style "description => code" is that you can use https://editor.swagger.io/ and other tools to edit the swagger (also possible offline),
which allows you to focus on making it look useful to the consumers, and yourself and your team, rather than hoping that "source => docs" will be somewhat consumable.

The generated code from the swagger definitions then allows you to "fill in the blanks" in such https://github.com/twilio/guardrail/blob/master/modules/sample/src/test/scala/core/RoundTrip.scala#L36 PetHandler type (which is from the typical PetStore example swagger file). It's style may be up to personal preference, however it allows being rather concise, and stick to what was defined in the swagger files rather well.

We are also aware of "Swakka" that takes the code => doc approach, however I have not investigated it too deep so far.
Link for reference: https://github.com/jtownson/swakka

Having that said, we do not currently maintain or support any swagger integration directly.
I would be rather interested in learning about what requirements you have, like: is swagger => code with fill-in-the-blanks suited for you, would you need java api etc.

Hope this helps at least a bit, thanks for additional context if you'd like to provide some - thanks!

# Observability

[https://medium.com/observability/microservices-observability-26a8b7056bb4](https://medium.com/observability/microservices-observability-26a8b7056bb4)

# Why I am seeing a StreamSupervisor  Error while using Akka Streams

bq. It's not clear to me where the StreamSupervisor instances come from and whether having so many is normal operation. Can you help me understand that?

A @StreamSupervisor@ is initialized each time you start an @ActorMaterializer@. Usually, a system will have a few of them, but certainly not that much. At this point, it's hard to say if it's something in your application that is initializing them or if one of the underlying libraries is causing that. Can you search in your code base for calls to @ActorMaterializer.create@ or simply @ActorMaterializer()@ (if using Scala)? An @ActorMaterializer@ internally creates a @StreamSupervisor@ that is referenced by the @ActorSystem@. They won't be GC'ed unless explicitly removed. The recommended pattern is to create one and reuse it. 

If you are interested, we have some more detailed information here: "https://doc.akka.io/docs/akka/snapshot/stream/stream-flows-and-basics.html#actor-materializer-lifecycle":https://doc.akka.io/docs/akka/snapshot/stream/stream-flows-and-basics.html#actor-materializer-lifecycle

Let me know if this solves the issue.

[Common Reasons Kubernetes Deployments Fail](./Common-Reasons-Kubernetes-Deployments-Fail-40f215ee-0412-446d-8a9c-35767b5e7bc1.md)

[Cinnamon with Datadog on Kubernetes](./Cinnamon-with-Datadog-on-Kubernetes-97b48f69-77ed-4fa8-b083-c3bcf9037195.md)
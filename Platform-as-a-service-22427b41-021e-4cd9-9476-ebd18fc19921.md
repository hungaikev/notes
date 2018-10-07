# Platform as a service

# REST

RESTful APIs uses HTTP as its transport protocol. For such cases, lookups should use GET requests. PUT, POST, and DELETE requests should be used for mutation, creation, and deletion respectively (avoid using GET requests for updating information)

motto is: “Quality, Speed, Joy”.

I think a big part of the problem is that we—as an industry—are not very good about thinking about how to make engineers effective.

Building things right will let you go faster. Building faster will give you more time to experiment and find your way to the right thing. And everybody enjoys building good stuff and a lot of it.

Obviously to save everyone five minutes a day, every day, we have to be working on something that everyone uses all the time.

How easy that is depends, obviously, on how much down time is currently induced by your tools and processes. It’s also possible to save time by removing things that will periodically eat larger amounts of time. An extra hour spent every couple weeks debugging a problem due to confusing error messages or logs is equivalent to five minutes a day and thus 1% of your total time in the year.

until you have people whose job it is to work on them, tools tend to be something hacked together just well enough to get something done.

Running Akka Applications. - Manually-crafted resource files are very brittle and easy to make mistakes.

but also thinking about what it takes to actually provide world-class tools and support to Twitter engineers.

Why could Twitter scale our software so well yet have so much trouble with the software we built for ourselves to help us do our jobs?

# What principles do we want to build on.

1. Paved Road - Anyone developing an Akka Application can get up and running in no time. 
2. Hiding Complexity (Docker files, Yaml files, Optimizing containers)
3. Self Serve

DevOps Tools

Component Manager 

Provisioning

Packaging and Update Manager

Usage/Metering Service

Using Helm Charts 

## User Stories

## 1. Application Deployment

## 1.1 Local Deployment

I'm a developer with a Akka/Lagom/Play project. I want to deploy to Minikube and see my project running locally by running a small set of commands (one command if possible) without manually generating/customizing configuration files.

## 1.2 Production Deployment

## 1.2.1 Developer Experience

I'm a developer with a Akka/Lagom/Play project. I want to build my application as a docker image and push it to my company's Docker registry, by running a small sets of commands from the terminal. I will then ask my operations person to deploy this image into production.

## 1.2.2 Operations Experience

I'm an operations person. I have an image in a Docker registry that I wish to deploy into my Kubernetes cluster. I will invoke (a) CLI command(s) with the name of this image which will deploy the application to the k8s cluster.

## 2. Application Upgrade

## 2.1 Rolling Upgrade

## 2.1.1 Developer Experience

I'm a developer with a Akka/Lagom/Play project. I want to create a new build of my application as a docker image and push it to my company's Docker registry, by running a small set of commands from the terminal. I will then ask my operations person to invoke a rolling upgrade of the application to this new version.

## 2.1.2 Operations Experience

I'm an operations person. I have a new version of an image in a Docker registry that I wish to deploy into my Kubernetes cluster. I will invoke (a) CLI command(s) with the name of this image which will perform a rolling upgrade of the application to the k8s cluster. During this process, instances will be swapped out for newer versions in a sequential fashion.

## 2.1 Blue/Green Deployment

## 2.1.1 Developer Experience

I'm a developer with a Akka/Lagom/Play project. I want to create a new build of my application as a docker image and push it to my company's Docker registry, by running a small set of commands from the terminal. I will then ask my operations person to invoke a blue/green deployment of the application to this new version, which will result in two sets of my application. Traffic is then directed from the old version to the new.

## 2.1.2 Operations Experience

I'm an operations person. I have a new version of an image in a Docker registry that I wish to deploy into my Kubernetes cluster. I will invoke (a) CLI command(s) with the name of this image which will perform a blue/green deployment of the application to the k8s cluster. During this process, a new set of instances will be spun up, and traffic will then be directed from the old version to the new.

## 3. Service Discovery

## 3.1 Service Discovery Through DNS SRV

I'm a developer. I’d like my application to be able to connect to other services via service discovery mechanism which supports DNS SRV.

## 3.2 Service Discovery Through DNS/IP (Legacy)

I'm a developer. I’d like my application to be able to connect to other services via service discovery mechanism which supports DNS/IP (Legacy).

## 3.3 Service Discovery Through External Services

I'm a developer. I’d like my application to be able to connect to other services via service discovery mechanism which supports locating external services.

## 3.4 Service Discovery Through ReactiveLib

I’m a developer with a Akka/Lagom/Play project. I’d like to include a library to simplify the service discovery lookup in my application. My application will leverage the library to lookup services via any of the above options

## 4. Privileged Containers

I’m a developer with a Akka/Lagom/Play project. I wish to indicate that my application requires elevated privileges in its build file, so that when it is deployed, it will be run in the proper context.

## 5. Startup

## 5.1 Coordinated Cluster Startup

I have an Akka/Lagom/Play application that requires startup in a coordinated fashion. I will use ReactiveLib to join or form an Akka Cluster. I assume that I am deploying to a K8s that is running ES Platform Services, which includes a Cluster Coordinator service

## 5.2 Non Cluster Startup

I have an Akka/Lagom/Play application that doesn’t require coordinated startup. This would not require the ES Cluster Coordinator Service to be running

## 9. Application Health

## 9.1 Specifying Criteria for Successful Startup of Application

## 9.1.1 Check Through File Creation

I’m a developer who will indicate to the platform that my reactive application has started up successfully by touching a file at a declared location that can be specified in my build file

## 9.1.2 Check Through Http

I’m a developer who will indicate to the platform that my reactive application has started up successfully by specifying the HTTP request details (e.g. port, path, method) in my build file

## 9.2 Specifying Criteria for Healthy Runtime of Application

## 9.2.1 Check Health Through Command

I’m a developer who will indicate to the platform that my reactive application is healthy by specifying a command that will indicate the health of my application.

## 9.2.2 Check Health Through Http

I’m a developer who will indicate to the platform that my reactive application is healthy by specifying the HTTP request details (e.g. port, path, method) in my build file. This can be the same or different as the startup check

## 9.3 Verify Successful Startup of Application

I’m a developer or operator who will check the status of successful startup of my application by using the platform commands (e.g. kubectl)

## 9.4 Verify Healthy Runtime of Application

I’m a developer or operator who will check the health of my application by using the platform commands (e.g. kubectl)

## 10. Application Availability

## 10.1 Singleton Application

I’m a developer. I’d like to specify a single instance of a particular application to be made available at any given time by the platform through my build file.

## 10.2 Application with placement constraints

I’m an operator. I’d like to have a particular application to be made available on nodes which match one or more placement constraints, i.e. (all nodes, nodes labelled with us-east-1a).

The user stories specified by 11. Deployment Metadata might be used to supply/override placement constraints information.

## 10.3 Multiple application instances

I’m an operator. I’d like to specify a specific number of instances of a particular application to be made available at any given time by the platform by using platform’s command line tool.

## 11. Deployment Metadata

## 11.1 Supplying deployment metadata as a developer

I’m a developer, I’d like to supply metadata which describes deployment concerns of my application via the build file.

## 11.2 Overriding deployment metadata as an operator

I’m an operator. I’d like to override the supplied metadata which describes deployment concerns of the applications supplied to me.

## 12. Platform Config & Secrets Management

## 12.1 Platform (s8s) Secrets Metadata Integration

As a developer, I should be able to make use of the login credentials, certificates, etc. that is stored by the underlying platform (i.e. k8s secrets) in my application to access services that my app depends on. For example, I may build an application that makes use of IBM’s chat service, for which I may need an API key and secret to access. This would be stored by Kubernetes as a Secrets object, whose information would be exposed to my application in a conveniently accessible way. One possible option (though not a strict requirement) is through ReactiveLib

## 12.2 Platform (k8s) Configuration Meta Integration

As a developer, I should be able to make use environment specific configuration variables/values, etc. that is stored by the underlying platform (i.e. k8s ConfigMap). For example, I may build an application that makes use of a threadpool who’s size will be configured differently depending on which k8s cluster I deploy to (e.g. staging vs prod). This information would be stored by Kubernetes as a ConfigMap object, whose information would be exposed to my application in a conveniently accessible way. One possible option (though not a strict requirement) is through ReactiveLib

## Additional User Stories (Unspecified)

- Application scale up/down
    - manual scaling
    - autoscaling (based on some application or load criteria)
- Security
    - application secrets
    - encryption of intraservice traffic
- configuration management
    - defining configuration sets/map
    - selecting configuration on deployment
    - env variables
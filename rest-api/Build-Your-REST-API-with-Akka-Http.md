# Build Your REST API with Akka Http

In this blog post, I will be discussing event sourcing, cqrs, kafka and how we are using it to build scalable data driven infrastructure. I am by no means an expert on the subject.

In this article, I will walk you through the step by step process for how to create a RESTful application using Akka HTTP and non-blocking IO at the backend with support for JSON request/response.

In this article, we are going to create RESTful apis including a POST and a GET APIs to serve Employee records from a database. There are 2 operations to going to handle in this application.

# **Use Cases**

Attendees

- Attendee can browse conferences
- Attendee can search for conferences
- Attendee can login
- Attendee can save a schedule for a conference
- Attendee is alerted to changes to a conference schedule

Speakers

- Speakers can find conferences with open calls for speakers/presentations
- Speakers can maintain a speaker profile
- Speakers can upload their presentations
- Speakers can submit presentations to conferences

Organizers

- Organizer can create a conference
- Organizer can add sessions
- Organizer can search for speaker presentations
- Organizer can create schedule

# **Use Case ‘Book movie tickets’**

1. System shows available Movies
2. User selects specific Movie
3. User selects a ‘Show’ for a specific movie
4. User ‘reserves seats’ for the selected Show
5. User enters contact details and confirms Order.
6. System shows payment options to user
7. User makes payment
8. After successful payment, system confirms Order and notifies user.

## What makes a good REST API?

There is a huge education gap in the ecosystem.

Nothing start to finish to act as a guide through the challenges of building a mature API with Akka Http.

The educational gap between beginner and the API professional remains vast. 

The landscape of the Web has changed significantly over the last few years, and REST APIs have seen massive growth. Today, most non-trivial applications need to expose an API at some point, and the trend is only going up.

APIs are also public, which means you need to get them right from the start, because they're very expensive to change.

Each blog post contains multiple, in-depth lessons along with walkthroughs, full code examples and resources, all helping you understand the inner working of building an API with Akka Http from initial architecture to deployment. 

## Learn by Building an Actual API

The lessons start with the fundamentals of building a practical REST API with Akka Http and quickly guides you through the more advanced tactics of a mature, well architected system.

But what makes REST with Spring unique is that you won't just learn how to ​create a well-designed API. You'll learn how to get that API into production, how to monitor it and how to make sure it stays up and running.

My aim is to show you exactly how to avoid common mistakes and get the API over the finish line.

The 13 modules cover building and securing the API for production use, advanced evolution and discovery techniques, client code to consume it from the front end, comprehensive monitoring, continuous integration, continuous deployment, and more.

In addition to the detailed guides on implementation, you’ll also get the knowledge needed to run the API in production reliably and consistently.

[1. The Basics of REST with Akka Http ](./1-The-Basics-of-REST-with-Akka-Http-125e9923-29a9-4afc-a491-3a52d25a2a67.md)

[2. REST and HTTP Semantics](./2-REST-and-HTTP-Semantics-44e64bd7-073d-4217-87d0-e79d89432077.md)

[3. Simple Security for REST](./3-Simple-Security-for-REST-71342fe5-fa27-4090-9bf0-86dc8711e904.md)

[4. Consuming the API ](./4-Consuming-the-API-36bda02f-1619-4968-a505-ec445ab23f72.md)

[5. Testing The API](./5-Testing-The-API-3601c54a-8488-407a-8a0f-5cd71dd964dd.md)

[6. Advanced API Security: OAuth2 and JWT](./6-Advanced-API-Security-OAuth2-and-JWT-3f8e57d3-c09d-4f34-a76a-eef2f7fa2e73.md)

[7. Document Discover & Evolve the REST API](./7-Document-Discover-Evolve-the-REST-API-16a8f8e3-3b8b-4626-97fa-a3f905367866.md)

[8. Monitoring & API Metrics](./8-Monitoring-API-Metrics-d69cc16c-3a18-4bf9-a318-311b2e402cd5.md)

[9. DevOps: CI and CD Pipelines, Deploment](./9-DevOps-CI-and-CD-Pipelines-Deploment-6fb3184b-c745-4faa-a074-ef1aaf5e96e0.md)

[10. Advanced API Tactics](./10-Advanced-API-Tactics-ea48af69-20b0-4175-bb1b-da7d1ef3d04f.md)

[11. Scaling Out with Akka Cluster](./11-Scaling-Out-with-Akka-Cluster-906e2a1f-6290-4b80-8044-275d8bdfb3eb.md)

[Exercises. ](./Exercises-e0abf231-c140-456c-ba45-54c9043596f0.md)
# Common Reasons Kubernetes Deployments Fail

# Common Reasons Kubernetes Deployments Fail

## 1. Wrong Container Image/Invalid Registry Permissions.

Most problems that occur  here are associated with 

- Having the wrong container image specified.
- Trying to use private images without providing registry credentials.

## Steps:

First, w'ell create a deployment named  `fail` pointing to a non-existent docker image: 

`kubectl run fail --image=nonexistent`

When you inspect the Pods we see we have one Pod with a status of `ErrImagePull` or `ImagePullBackOff`

`kubectl get pods` 

To get additional information, we can then describe the failing pod

`kubectl describe pod <pod_name>`

If you look in the Events section of the output of the describe command we see that Kubernetes was not able to find the image `image:nonexistent`

Why couldn't Kubernetes pull the image?

There are 3 common reasons besides network connectivity

- The image tag is incorrect
- The image does not exist (or is in a different registry)
- Kubernetes does not have permission to pull that image.

Try running `docker pull` on your local deployment machine with the exact same image tag. 

- If the exact image tag fails, then  that means the tag doesn't exist
- If `docker pull nonexistent` without a tag fails, then we have a bigger problem. The image does not exist at all in our image registry.

**Note:** There is no observable difference in Pod status between a missing image and incorrect permissions. In either case Kubernetes will report an `ErrImagePull` status for the pods

# 2. Application Crashing after Launch

Having an application crash on startup is a common occurrence. 

## Steps:

Create a new Deployment with an application that crashes after 1 second. 

- `kubectl run crasher --image="crashing-app"`

Take a look at the status of our Pods

- `kubectl get pods`

On the status section of our deployment we can see `CrashLoopBackOff` It tells us that Kubernetes is trying to launch this Pod but one or more of the containers is crashing or getting killed.  

Describe `describe` the pod to get more information. 

- `kubectl describe pod <pod_name>`

Kubernetes is telling us that this pod is being `Terminated` due to the application inside the container crashing. 

Why is our application crashing?

First the first thing to do is check our application logs. 

Assuming you are sending your application logs to `stdout` you can see the application logs using 

- `kubectl logs <pod_name>`

If the Pod does not seem to have any log data. It is possible we are looking at a newly restarted instance of the application, so we should check the previous container. 

- `kubectl logs <pod_name>  --previous`

If our application still is not giving us anything to work with it is probably time to add some additional log messages on startup to help debugging the issue. 

Try running the container locally to see if there are missing environmental variables or mounted volumes. 

# 3. Missing ConfigMap or Secret.

Kubernetes recommends passing application runtime configuration via `ConfigMaps` or `Secrets` 

Data could include database credentials, API endpoints, or other configuration flags. 

## Steps:

Try create a Pod that loads ConfigMap data ad environment variables. 

Get the Pods 

- `kubectl get pods`

Our pod status says `RunContainerError` 

Describe the pod to get more information. 

- `kubectl describe pod <pod_name>`

Check the `Events` section because it explains what went wrong. The Pod is attempting to access a `ConfigMap` but its not found in this namespace. After creating the `ConfigMap` the pod should restart and pull in the runtime data. 

# 4. Liveness/Readiness Probe Failures.

Essentially, Liveness/Readiness probes will periodically perform an action (eg make an HTTP request, open a TCP connection, or run a command in your container) to confirm that your application is working as intended. 

If the Liveness probe fails, Kubernetes will kill your container and create a new one. 

If the Readiness probe fails that Pod will not be available as a `Service` endpoints, meaning no traffic will be sent to that Pod until it becomes `Ready`

## Steps:

Define a Pod spec that defines a Liveness & Readiness probe that checks for a healthy HTTP response for `/heathlz` on port 8080

After a few minutes:

- `kubectl get pods`

After 2 minutes we can see that our Pod is still not `Ready` and it has been restarted a few times. 

Describe the Pod for more information: 

- `kubectl describe pod <pod_name>`

Check `Events` section. We can see that the Readiness and Liveness probes are both failing. 

There are 3 likely possibilities: 

- Your probes are now incorrect - Did the health URL change?
- Your probes are too sensitive - Does your application take a while to start or respond?
- Your application is no longer responding correctly to the probe - Is your database misconfigured?

Looking at the logs from your Pod is a good place to start debugging. 

# 5. Role Based Access (RBAC).

GCP enables Role-Based Access Control (aka RBAC) by default in new Kubernetes clusters. This requires configuring roles to allow access to resources within the Kubernetes API. Lightbend Orchestration (specifically, the Akka Cluster Bootstrap component) uses the Kubernetes API to automatically form a cluster. Without binding your pods' service account to a role that grants access to the required resources, these API calls fail.

# 6. Insufficient Cluster Resources.

If you do all your work in the `default` namespace

- `kubectl describe ns default`

[https://github.com/coreos/awesome-kubernetes-extensions](https://github.com/coreos/awesome-kubernetes-extensions) 

[https://github.com/lightbend/akka-commercial-addons/issues/340](https://github.com/lightbend/akka-commercial-addons/issues/340) 

[https://github.com/akka/akka-management/issues/134](https://github.com/akka/akka-management/issues/134) 

[https://kubernetes.io/docs/tasks/access-kubernetes-api/extend-api-third-party-resource/](https://kubernetes.io/docs/tasks/access-kubernetes-api/extend-api-third-party-resource/)

[https://coreos.com/operators/](https://coreos.com/operators/)
# Akka Cluster Fails

## Blog:: Kubernetes Secrets with Lightbend config

The distinction between a Kubernetes cluster (of underlying hosts) and the Akka cluster(s) running on top of it. (These don’t match one-to-one.)

Specific clarification of the distinction between service-level clustering (Akka/Lagom) and infrastructure-level clustering (k82/Mesos/ECS), including clarification that members of multiple independent, logical service clusters might be colocated on a single infrastructure node. 

one cluster == one codebase == one deployment cycle

One cluster per service vs. a cluster that spans services (using Akka messaging between services)

Recommendations (and rationale) for different approaches for internal service communication between nodes vs. communication between services vs. external communication with clients such as browsers, mobile apps, third-party API clients, etc.

## Cluster Roles

    I have set up a cluster singleton actor as following:
    
    
    
    val assemblerSingletonActor = system.actorOf(
    ClusterSingletonManager.props(
    singletonProps = Props(classOf[AssemblerSingletonActor]),
    terminationMessage = TerminateSingletonActor,
    settings = new ClusterSingletonManagerSettings(
    singletonName = "assemblerSingletonActor",
    role = None,//Some("subscribe"),
    removalMargin = 5 seconds,
    handOverRetryInterval = 1 seconds
    )
    ),
    name = "assemblerSingletonManager"
    )
    
    val assemblerSingletonProxyActor = system.actorOf(
    ClusterSingletonProxy.props(
    singletonManagerPath = "/user/assemblerSingletonManager",
    settings = new ClusterSingletonProxySettings(
    singletonName = "assemblerSingletonActor",
    role = Some("subscribe"),
    singletonIdentificationInterval = 1 seconds,
    bufferSize = 1000
    )
    ),
    name = "assemblerSingletonProxyActor"
    )
    ClusterClientReceptionist(system).registerService(assemblerSingletonProxyActor)
    
    
    when sending message to the proxy actor like: 
    
    assemblerSingletonProxyActor ! CompletedMsg("test" , 1 ,2)
    
    I got the following debug output and the singleton actor didn't receive the message:
    [DEBUG] [03/10/2017 16:06:48.991] [DimensionAssemblerSystem-akka.actor.default-dispatcher-5] [akka.tcp://DimensionAssemblerSystem@127.0.0.1:2552/user/assemblerSingletonProxyActor] Singleton not available, buffering message type [assembler.message.CompletedMsg]
    [INFO] [03/10/2017 16:06:49.095] [DimensionAssemblerSystem-akka.actor.default-dispatcher-20] [akka.tcp://DimensionAssemblerSystem@127.0.0.1:2552/user/assemblerSingletonManager] Singleton manager starting singleton actor [akka://DimensionAssemblerSystem/user/assemblerSingletonManager/assemblerSingletonActor]
    [INFO] [03/10/2017 16:06:49.097] [DimensionAssemblerSystem-akka.actor.default-dispatcher-20] [akka.tcp://DimensionAssemblerSystem@127.0.0.1:2552/user/assemblerSingletonManager] ClusterSingletonManager state change [Start -> Oldest]
    
    
    If I send message to the clusterSingletonManager like this:
    
    
    assemblerSingletonActor ! CompletedMsg("test" , 1 ,2)
    
    
    then I got this warning:
    
    [WARN] [03/10/2017 16:09:53.876] [DimensionAssemblerSystem-akka.actor.default-dispatcher-15] [akka.tcp://DimensionAssemblerSystem@127.0.0.1:2552/user/assemblerSingletonManager] unhandled event CompletedMsg(test,1,2) in state Start
    [INFO] [03/10/2017 16:09:53.953] [DimensionAssemblerSystem-akka.actor.default-dispatcher-17] [akka.tcp://DimensionAssemblerSystem@127.0.0.1:2552/user/assemblerSingletonManager] Singleton manager starting singleton actor [akka://DimensionAssemblerSystem/user/assemblerSingletonManager/assemblerSingletonActor]
    [INFO] [03/10/2017 16:09:53.954] [DimensionAssemblerSystem-akka.actor.default-dispatcher-17] [akka.tcp://DimensionAssemblerSystem@127.0.0.1:2552/user/assemblerSingletonManager] ClusterSingletonManager state change [Start -> Oldest]

you need to make sure that the configuration of manager and proxy match. 
In your case, it seems you defined different values for the `role` setting 
for manager and proxy. 
The role determines which nodes in your cluster the singleton can run on. 
If you want to configure roles for nodes in the cluster this is something that needs to be done in the cluster 
configuration (but I would suggest to get it to work without roles).

I would suggest to start from a simple example without changing the configuration at all before tweaking settings:

    val assemblerSingletonActor = system.actorOf( 
    ClusterSingletonManager.props( 
    singletonProps =
    Props(classOf[AssemblerSingletonActor]), 
    terminationMessage = TerminateSingletonActor, 
    settings = ClusterSingletonManagerSettings(system)),
    name = "assemblerSingletonManager" 
    )
    
    val assemblerSingletonProxyActor = system.actorOf(
    ClusterSingletonProxy.props( 
    singletonManagerPath = "/user/assemblerSingletonManager", 
    settings = ClusterSingletonProxySettings(system)
    ), 
    name = "assemblerSingletonProxyActor" 
    )

You can access the singleton only through the proxy (not through the manager). So, this should work:

`assemblerSingletonProxyActor ! CompletedMsg("test" , 1 ,2)`

The logs you posted show that the proxy received the message at a time the cluster was not yet fully formed or had not converged yet, so the singleton node was not yet determined. Probably because of the wrong `role` setting, the proxy did not figure out where to send that message.

Could you try if it works with the simplified configuration?

## Network Issues

    [][04 Jan 2018 00:56:36.222][WARN][ReliableDeliverySupervisor][?][Thread ClusterSystem-akka.actor.default-dispatcher-63] - Association with remote system [akka.tcp://ClusterSystem@10.172.74.3:42494] has failed, address is now gated for [5000] ms. Reason: [Disassociated]
    [][04 Jan 2018 00:56:44.972][INFO][ProtocolStateActor][?][Thread ClusterSystem-akka.actor.default-dispatcher-51] - No response from remote. Transport failure detector triggered. (internal state was Open)
    [][04 Jan 2018 00:56:44.972][WARN][ReliableDeliverySupervisor][?][Thread ClusterSystem-akka.actor.default-dispatcher-51] - Association with remote system [akka.tcp://ClusterSystem@10.172.16.25:34861] has failed, address is now gated for [5000] ms. Reason: [Disassociated]

usually these kind of issues are caused by intermittent reachability issues. It means that an ActorSystem currently cannot reach another an ActorSystem on another node and will wait for the given time interval before retrying communication with that node. Usually it will self-heal after a while when the other ActorSystem is reachable again. There are many potential causes for an ActorSystem to become unreachable. A common one is intermittent networking issues. The other is that the other ActorSystem doesn't response because it is busy. That can be because of long GC pauses or because of thread starvation. Make sure to run the Thread Starvation Detector which will point out potential errors early on.

Was the communication resumed after a while? Akka usually generates quite a bit of log messages regarding remote and cluster connections. If there is only little logging from Akka you might consider changing the log level of Akka logging.

In any case, intermittent connection outages are an expected condition of every production system. Akka guarantees at-most-once message delivery. That means that especially in cases of network outages messages might be lost. You need to make sure your application can deal with that, e.g. by resending idempotent message or by using timeouts.

The most common cause of unreachability is thread starvation or overload on the remote node. It means that all of Akka's threads are blocking or the remote ActorSystem becomes too slow for whatever reason to answer to internal system messages in time.

Another common cause are intermittent networking issues.

Also note, that unreachability is not fatal. If the cause for the unreachability is intermittent, the connection will be attempted to be reestablished automatically periodically.

## Seed Nodes

A multi seed node cluster must have each of the seed nodes listed in the config, on all of the nodes, in the same order. So for a cluster with three seed-nodes that would be:

    seed-nodes = [
          "akka.tcp://CluseterName@seed-host-1:4500",
          "akka.tcp://CluseterName@seed-host-2:4500",
          "akka.tcp://CluseterName@seed-host-3:4500",
        ]

How Akka behaves with multiple seed nodes is that when a node joins the cluster, it will send a join-request to all of the listed seeds, if none of them answers, and the node itself is the first in the list, it will join itself and thus form a cluster, having more than one is important since if the single seed node configuration that node crashes and restarts it will always join itself and form a new cluster, and never be able to join the existing cluster even if there are other nodes still alive.

In a multi-seed node config, when the first seed node crashes and restarts it will first try to join the other seed nodes in the list, and not join itself until it notices that those nodes are not alive.

You must make sure the seed node list is sorted the same on each started node, if it isn't this will lead to multiple isolated clusters forming.

## Failure Detector

Note that things like GC, CPU overload, and thread starvation (blocking of default dispatcher) may also cause FD to trigger.

Keep an eye on GC pauses and for the "heartbeat interval is growing too large" message though this seems like a good start.

To detect network problems and crashed nodes. If the heartbeat messages (requrest-reply) can’t get through (lost or delayed) it will mark them as Unreachable. When heartbeats can get through again it will be marked as reachable again. This doesn’t mean that nodes are removed from the cluster membership.

To decide when an Unreachable node should be removed from the cluster it has to be Downed. That is done by a downing provider, such as Lightbend’s Split Brain Resolver 1, or manually with Cluster management tool. 

Some cluster tools, such as Cluster aware routers, use the reachability information to avoid routing messages to unreachable nodes.

The default size of the system messages bufffer is 20000 and it can be increased with configuration property `akka.remote.artery.advanced.system-message-buffer-size`. There is no drawback apart from possible memory consumption to increase this. The buffer is an `ArrayDeque` so it grows as needed, but doesn’t shrink.

There is also another queue for outgoing control (system) messages and the max size of that is configured with `akka.remote.artery.advanced.outbound-control-queue-size`. The default is `3072`. This is a `LinkedBlockingQueue` so it’s also ok to increase. I think we should increase the default of this, by the way.


### Akka Remote
Remote deployment is not supported, including cluster aware pool routers. The reasons why it's not supported:

 1. It encourages an application design that is rather brittle. If the parent crashes it brings down all remote deployed childrens. That can be seen as a good feature, but often it probably used without realizing the consequences of the tight coupling.
1. It introduces much complexity and special cases in the internals of Akka.
It should be replaced by ordinary actor messaging protocols, such as a manager actor running on each node that is told to spawn children for certain tasks. The Cluster receptionist is useful for this interaction.

This is not a complete description, but a start.







 Akka relies on heartbeats to detect that a node is unreachable. Heartbeats can be lost or delayed for lots of reasons like network connectivity and stability issues and garbage collection times.
 
 In the case of Akka Persistence (and Cluster Singletons), failover times would mean, how long it takes that persistent actors or singletons are migrated from a node that has been downed to a healthy node. To estimate that time it makes sense to see what is involved in this process:

1. Node(s) gets unavailable (network, GC, crashed, or something else)
1. Other nodes in the cluster detect that situation by failing heartbeats. A failure detector eventually flags the node as unreachable.
1. SBR listens to updates of the current cluster state and eventually receives information that a node was marked as unreachable.
1. SBR waits that no updates happen for the stable-after time.
1. SBR takes a decision and acts to down nodes and remove them from the cluster.
1. Shard coordinators migrate persistent actors to another node
1. Persistent actors start up at the new node.
1. Migrated persistent actors load snapshots and/or replay events to load latest state.

All of these steps will contribute to the total failover time. A clean crash of a node might run through these steps faster than if there's an unstable network condition where reachability is unstable for a while. With the usually recommended settings the whole process may take tens of seconds to minutes. You can tweak the settings for the process to run faster but you will have to run your own tests to see if this will end in unnecessary restarts (which make availability worse).

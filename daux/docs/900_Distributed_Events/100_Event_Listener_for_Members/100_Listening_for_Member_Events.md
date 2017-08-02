

You can register for Hazelcast entry events so you will be notified when those events occur. Event Listeners are cluster-wide--when a listener is registered in one member of cluster, it is actually registered for events that originated at any member in the cluster. When a new member joins, events originated at the new member will also be delivered.

An Event is created only if you registered an event listener. If no listener is registered, then no event will be created. If you provided a predicate when you registered the event listener, pass the predicate before sending the event to the listener (member/client).

As a rule of thumb, your event listener should not implement heavy processes in its event methods that block the thread for a long time. If needed, you can use `ExecutorService` to transfer long running processes to another thread and thus offload the current listener thread.

![image](../../images/NoteSmall.jpg) ***NOTE:*** *In a failover scenario, events are not highly available and may get lost. Eventing mechanism is being improved for failover scenarios.*


Hazelcast offers the following event listeners:

- **Membership Listener** for cluster membership events.
- **Distributed Object Listener** for distributed object creation and destroy events.
- **Migration Listener** for partition migration start and complete events.
- **Partition Lost Listener** for partition lost events.
- **Lifecycle Listener** for `HazelcastInstance` lifecycle events.
- **Entry Listener** for `IMap` and `MultiMap` entry events.
- **Item Listener** for `IQueue`, `ISet` and `IList` item events.
- **Message Listener** for `ITopic` message events.
- **Client Listener** for client connection events.


### Listening for Member Events


The Membership Listener interface has methods that are invoked for the following events.

- `memberAdded`: A new member is added to the cluster.
- `memberRemoved`: An existing member leaves the cluster.
- `memberAttributeChanged`: An attribute of a member is changed. Please refer to [Defining Member Attributes](/17_Management/03_Cluster_Utilities/05_Defining_Member_Attributes.md) to learn about member attributes.

To write a Membership Listener class, you implement the MembershipListener interface and its methods.

The following is an example Membership Listener class.

```java
public class ClusterMembershipListener
     implements MembershipListener {
     
public void memberAdded(MembershipEvent membershipEvent) {
  System.err.println("Added: " + membershipEvent);
}

public void memberRemoved(MembershipEvent membershipEvent) {
       System.err.println("Removed: " + membershipEvent);
     }

public void memberAttributeChanged(MemberAttributeEvent memberAttributeEvent) {
       System.err.println("Member attribute changed: " + memberAttributeEvent);
     }
     
}
```

When a respective event is fired, the membership listener outputs the addresses of the members that joined and left, and also which attribute changed on which member.

#### Registering Membership Listeners

After you create your class, you can configure your cluster to include the membership listener. Below is an example using the method `addMembershipListener`.

```java
HazelcastInstance hazelcastInstance = Hazelcast.newHazelcastInstance();
hazelcastInstance.getCluster().addMembershipListener( new ClusterMembershipListener() );
```

With the above approach, there is the possibility of missing events between the creation of the instance and registering the listener. To overcome this race condition, Hazelcast allows you to register listeners in the configuration. You can register listeners using declarative, programmatic, or Spring configuration, as shown below.

The following is an example programmatic configuration.

```java
Config config = new Config();
config.addListenerConfig(
new ListenerConfig( "com.your-package.ClusterMembershipListener" ) );
```


The following is an example of the equivalent declarative configuration. 

```xml
<hazelcast>
   ...
   <listeners>
      <listener>
         com.your-package.ClusterMembershipListener
      </listener>
   </listeners>
   ...
</hazelcast>
```

The following is an example of the equivalent Spring configuration.

```
<hz:listeners>
 <hz:listener class-name="com.your-package.ClusterMembershipListener"/>
 <hz:listener implementation="MembershipListener"/>
</hz:listeners>
```


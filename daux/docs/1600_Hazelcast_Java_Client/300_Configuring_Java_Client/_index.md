
You can configure Hazelcast Java Client declaratively (XML) or programmatically (API).

For declarative configuration, the Hazelcast client looks at the following places for the client configuration file.

- **System property**: The client first checks if `hazelcast.client.config` system property is set to a file path, e.g., `-Dhazelcast.client.config=C:/myhazelcast.xml`.

- **Classpath**: If config file is not set as a system property, the client checks the classpath for `hazelcast-client.xml` file.

If the client does not find any configuration file, it starts with the default configuration (`hazelcast-client-default.xml`) located in the `hazelcast-client.jar` library. Before configuring the client, please try to work with the default configuration to see if it works for you. The default should be just fine for most users. If not, then consider custom configuration for your environment.

If you want to specify your own configuration file to create a `Config` object, the Hazelcast client supports the following.

- `Config cfg = new XmlClientConfigBuilder(xmlFileName).build();`

- `Config cfg = new XmlClientConfigBuilder(inputStream).build();`


For programmatic configuration of the Hazelcast Java Client, just instantiate a `ClientConfig` object and configure the desired aspects. An example is shown below.

```java
ClientConfig clientConfig = new ClientConfig();
clientConfig.setGroupConfig(new GroupConfig("dev","dev-pass”);
clientConfig.setLoadBalancer(yourLoadBalancer);
...
...
```

### Configuring Client Near Cache


The Hazelcast distributed map supports a local Near Cache for remotely stored entries to increase the performance of local read operations. Since the client always requests data from the cluster members, it can be helpful in some use cases to configure a Near Cache on the client side. Please refer to the [Near Cache section](/19_Performance/04_Near_Cache) for a detailed explanation of the Near Cache feature and its configuration.


### Configuring Client Groups


Clients should provide a group name and password in order to connect to the cluster.
You can configure them using `GroupConfig`, as shown below.

```java
clientConfig.setGroupConfig(new GroupConfig("dev","dev-pass"));
```


### Configuring Client Security


In the cases where the security established with `GroupConfig` is not enough and you want your clients connecting securely to the cluster, you can use `ClientSecurityConfig`. This configuration has a `credentials` parameter to set the IP address and UID. Please see `ClientSecurityConfig.java` in our code.

### Configuring Client Serialization

For the client side serialization, use Hazelcast configuration. Please refer to the [Serialization chapter](/16_Serialization).

### Configuring Executor Pool Size



Hazelcast has an internal executor service (different from the data structure *Executor Service*) that has threads and queues to perform internal operations such as handling responses. This parameter specifies the size of the pool of threads which perform these operations laying in the executor's queue. If not configured, this parameter has the value as **5 \* *core size of the client*** (i.e. it is 20 for a machine that has 4 cores).

### Configuring Class Loaders


You can configure a custom `classLoader`. It will be used by the serialization service and to load any class configured in configuration, such as event listeners or ProxyFactories.


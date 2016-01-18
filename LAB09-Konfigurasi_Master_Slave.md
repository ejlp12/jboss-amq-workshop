# DEPLOY MASTER/SLAVE FAILOVER

We're now going to do a walk through of creating a local Master/Slave failover pair of JBoss A-MQ servers.

One of the ways JBoss A-MQ handles server faults is through the use of **Master/Slave (aka Active/Passive)** pairs of servers. The important part is that the Master/Slave nodes MUST reference the same ActiveMQ persistence store. 

To be clear, we are talking about the messages stored within an JBoss A-MQ persistent store. That is, messages that have been sent (produced) to a node waiting to be received (consumed). A message lives in one and only one JBoss A-MQ node at any point. So when a node is off (either as a controller shutdown or a system fault), its persistent messages are safely stored in its persistent store waiting for some nice JBoss A-MQ node to reference it and dispatch its messages...

JBoss A-MQ can do Fault Tolerance using other techniques (e.g. RHEL High Availability, or replicated stores), but that's beyond the scope of this lab...

Since we don't have a SAN (GFS2 or NFS v4) or a shared RDBMS available to us (“I know I just had it in my pocket the other day...”), we're going to do this lab with a shared local directory. In the real world, you would want your Master node and Slave node(s) on different machines to allow you to recover from machine failure. The default configuration (see Lab 06 - ActiveMQ Configuration) uses a file based persistent store (KahaDB). So if we create two containers that reference the same configuration (with the shared reference to the same directory), we should be good...

Make sense? Let's try and see if that helps...

When you first start as a Fabric, the root node actually does have a broker in it. You can tell this by seeing the little Broker icon in the far right of the column. We’ll want to set up fault-tolerant pair of brokers, so let’s remove the existing one. 

![image](https://cloud.githubusercontent.com/assets/3068071/12401199/90a7a6f6-be57-11e5-8309-788fdc7e86e6.png)

At the next screen, you should see the list of profiles that have been installed on that container. Remove the “amq” profile.

![image](https://cloud.githubusercontent.com/assets/3068071/12401218/a5b54544-be57-11e5-969c-102a14f13fd8.png)

After the “amq” profile has been removed, the container will analyze itself, provision, and remove the broker. This may result in you needing to log back in.

Click back to the _Services_ tab, and click “MQ”

![image](https://cloud.githubusercontent.com/assets/3068071/12401232/c4ff8a90-be57-11e5-9e74-937b867c6add.png)

At the MQ tab, click “Create Broker Configuration”

![image](https://cloud.githubusercontent.com/assets/3068071/12401267/e41479f4-be57-11e5-9657-789e39f8cc0d.png)

Now we’ll need to configure a broker. You can keep the default group if you’d like. Just give it a name of “tolerantbroker”, and choose “MasterSlave” for the type of broker. You can keep all the defaults for the other settings and press “Create Broker”. 

> Note: this does not actually create a running broker, just a configuration profile that describes the broker.

![image](https://cloud.githubusercontent.com/assets/3068071/12401280/03f4d14c-be58-11e5-9883-6f3649b35646.png)

Now we can see that our broker has been created, but there are no containers running it. So there are a few red error indicators. Click on one of them to create some containers that will run this broker configuration.


![image](https://cloud.githubusercontent.com/assets/3068071/12401295/206973a0-be58-11e5-97f0-11ab5d6724d0.png)

Give the container a name. The profile specifies “2” as the number of containers to create. So leave the default and click “Create and Start Container”


![image](https://cloud.githubusercontent.com/assets/3068071/12401317/321413c6-be58-11e5-9925-39a53e285a73.png)

And that’s it! We now have two brokers running, one is a master and one is a slave. We can see them listed in the containers list (the master broker has the little broker icon off to the right) as well as in the Services->MQ tab. Click it and guess which one is the master. :)

![image](https://cloud.githubusercontent.com/assets/3068071/12401426/d237f9d0-be58-11e5-8153-0063a49403e5.png)

## Test

Now let’s test out our clients again.

Open up terminal window, and run the following command:

```
java -jar extras/mq-client.jar producer \
     --brokerUrl discovery:fabric:default \
     --user admin --password admin \
     --destination queue://workshop.test \
     --count 10
```

Notice that we are now giving a brokerUrl of `discovery:fabric:default`. This tells the JBoss A-MQ client library to use its pluggable discovery mechanism to look up the connection information for these JBoss A-MQ nodes from the Fabric registry on the local machine (we'd need to provide a system property `-Dzookeeper.url=<host>:<port>` if we wanted to connect to a Fabric Registry on a different machine). You should see the following result:

```
$java -jar extras/mq-client.jar producer --brokerUrl discovery:fabric:default --user admin --password admin --destination queue://workshop.test --count 10
Using destination: queue://workshop.test, on broker: discovery:fabric:default
[io.fabric8.mq.fabric.discovery.FabricDiscoveryAgent] : Using local ZKClient
[org.apache.curator.framework.imps.CuratorFrameworkImpl] : Starting
[org.apache.zookeeper.ZooKeeper] : Client environment:zookeeper.version=3.4.6-1569965, built on 02/20/2014 09:09 GMT
[org.apache.zookeeper.ZooKeeper] : Client environment:host.name=localhost.localdomain
[org.apache.zookeeper.ZooKeeper] : Client environment:java.version=1.8.0_45
[org.apache.zookeeper.ZooKeeper] : Client environment:java.vendor=Oracle Corporation
[org.apache.zookeeper.ZooKeeper] : Client environment:java.home=/opt/java/jdk1.8.0_45/jre
[org.apache.zookeeper.ZooKeeper] : Client environment:java.class.path=extras/mq-client.jar
[org.apache.zookeeper.ZooKeeper] : Client environment:java.library.path=/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
[org.apache.zookeeper.ZooKeeper] : Client environment:java.io.tmpdir=/tmp
[org.apache.zookeeper.ZooKeeper] : Client environment:java.compiler=<NA>
[org.apache.zookeeper.ZooKeeper] : Client environment:os.name=Linux
[org.apache.zookeeper.ZooKeeper] : Client environment:os.arch=amd64
[org.apache.zookeeper.ZooKeeper] : Client environment:os.version=3.19.8-100.fc20.x86_64
[org.apache.zookeeper.ZooKeeper] : Client environment:user.name=jreagan
[org.apache.zookeeper.ZooKeeper] : Client environment:user.home=/home/jreagan
[org.apache.zookeeper.ZooKeeper] : Client environment:user.dir=/home/jreagan/Development/JBoss_A-MQ/jboss-a-mq-6.2.0.redhat-133
[org.apache.zookeeper.ZooKeeper] : Initiating client connection, connectString=localhost:2181 sessionTimeout=60000 watcher=org.apache.curator.ConnectionState@29a0e378
[org.apache.zookeeper.ClientCnxn] : Opening socket connection to server localhost.localdomain/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
[org.apache.zookeeper.ClientCnxn] : Socket connection established to localhost.localdomain/127.0.0.1:2181, initiating session
[org.apache.zookeeper.ClientCnxn] : Session establishment complete on server localhost.localdomain/127.0.0.1:2181, sessionid = 0x14f245607720006, negotiated timeout = 40000
[org.apache.curator.framework.state.ConnectionStateManager] : State change: CONNECTED
[org.apache.activemq.transport.discovery.DiscoveryTransport] : Adding new broker connection URL: tcp://127.0.0.1:41662
[org.apache.activemq.transport.discovery.DiscoveryTransport] : Adding new broker connection URL: tcp://127.0.0.1:61617
[org.apache.activemq.transport.failover.FailoverTransport] : Successfully connected to tcp://127.0.0.1:41662
[io.fabric8.mq.ProducerThread] : Thread-0 Started to calculate elapsed time ...

[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 0
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 1
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 2
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 3
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 4
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 5
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 6
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 7
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 8
[io.fabric8.mq.ProducerThread] : Thread-0 Sent: test message: 9
[io.fabric8.mq.ProducerThread] : Thread-0 Produced: 10 messages
[io.fabric8.mq.ProducerThread] : Thread-0 Elapsed time in second : 0 s
[io.fabric8.mq.ProducerThread] : Thread-0 Elapsed time in milli second : 474 milli seconds
[io.fabric8.mq.ActiveMQService] : Closed JMS connection
[[org.apache.zookeeper.ClientCnxn] : EventThread shut down
org.apache.zookeeper.ZooKeeper] : Session: 0x14f245607720006 closed
[io.fabric8.mq.ProducerThread] : Thread-0 Producer thread finished
All threads completed their work
```


## Test 2

Now try running your producer with a larger count (e.g. `--count 1000`) and a delay (`--sleep 1000`). Then try stopping and starting the broker1 and broker2 containers to see what happens...

You can use the command line Console to tell you the current Master and Slave(s) pairs for any Fabric by running the `fabric:cluster-list` command (you might want to give it a path of amq/default to narrow down the results).

You should see the following result:

```
JBossA-MQ:karaf@root> fabric:cluster-list amq/default
[cluster]       [masters]        [slaves]          [services]     []
root            root             -                 tcp://127.0.0.1:61617
tolerantbroker  brokercontainer  brokercontainer2  tcp://127.0.0.1:41662, mqtt://127.0.0.1:57413, amqp://127.0.0.1:38335, stomp://127.0.0.1:55610
```


You can start and stop containers from the Containers tab in FMC, or from the Console with `fabric:container-{start,stop} <container name>`.

Be patient, it can take up a minute or so for the failover to happen (time to detect and convert Slave to Master, update the Registry with the new Master info, and failover the client connections...). Trust me this will happen way faster than you responding to a call at midnight and making your first cup of coffee...

Stand up and shout with joy “JBoss A-MQ makes me Fault Tolerant!”. Brings a tear to my eye; you were only just a newbie not that long ago...




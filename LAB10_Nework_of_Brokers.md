# LAB 10 - DEPLOY A NETWORK OF BROKERS

Now we're going to show how to **horizontally scale out** your JBoss A-MQ deployment by federating multiple broker instances together using JBoss A-MQ's **Network of Brokers** capabilities. 

This time we're going to **create two containers with different persistent stores and connect them together**. Additionally, we’re going to show how to use the command line for this. You can do it with the web console as well. Connecting JBoss A-MQ nodes together means that the nodes establish a Network Bridge between themselves such that they will forward messages for a given Destination (queue or topic) to consumers connected to any node within the federation. Under the covers we're using ActiveMQ's `networkConnectors` to help with this magic.

Let's create our federation. Run the following two (2) commands in you Fabric Controller.

```
fabric:mq-create --group a-mq-east \
     --kind StandAlone \
     --networks a-mq-west \
     --networks-username admin --networks-password admin \
     --create-container a-mq-east \
     a-mq-east-broker
```

```
fabric:mq-create --group a-mq-west \
     --kind StandAlone \
     --networks a-mq-east \
     --networks-username admin --networks-password admin \
     --create-container a-mq-west \
     a-mq-west-broker
```


This will create two (2) new Containers (a-mq-east and a-mq-west) with two (2) new Profiles (a-mq-east-broker and a-mq-west-broker). Each Container is running an embedded ActiveMQ instance which are networked together. That’s what the --networks setting is doing; connecting one group to another.

Let's try using it. Open two (2) terminal windows and run the following commands:

**Producer**
```
java -jar extras/mq-client.jar producer \
     --brokerUrl discovery:fabric:a-mq-east \
     --user admin --password admin \
     --destination queue://workshop.test \
     --count 10
```

**Consumer**
```
java -jar extras/mq-client.jar consumer \
     --brokerUrl discovery:fabric:a-mq-west \
     --user admin --password admin \
     --destination queue://workshop.test \
     --count 10
```


The messages are being sent (produced) to the **a-mq-east** broker, and being consumed from the **a-mq-west** broker, flowing through the JBoss A-MQ network connector.

Try different combinations of clients connected to different nodes. Then try starting and stopping containers to see what happens.

> Note: Network of Brokers is about federating or interconnecting nodes together to scale out your message processing capacity. It is not about fault tolerance (in that any messages in one broker's persistent store are not available to another broker unless the owning broker is running). JBoss A-MQ Master/Slave (Lab 09 - Deploy Master/Slave Failover) is one way to automate recovery (failover) of messages located within a given persistence store. 
>
> The purpose of a Network of Brokers is to help you with scale out (such as connecting JBoss A-MQ instances located in two different data centers). You can of course create Networks of Master/Slave pairs to get a more fault tolerant and scalable messaging infrastructure...

That's it for this whirlwind tour of JBoss A-MQ. If you enjoyed this lab, please shout out “A-MQ Rocks!”

For more information on JBoss A-MQ, and continue your journey, go to http://www.jboss.org/products/amq.


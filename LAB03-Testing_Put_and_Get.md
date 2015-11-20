# LAB 03 – TESTING JBOSS A-MQ FOR THE FIRST TIME

JBoss A-MQ includes a messaging test client as part of its distribution that helps you validate that everything is working correctly. You can download the client jar library (`mq-client-1.2.0.redhat-163.jar`) from [this site](https://repository.jboss.org/nexus/content/repositories/ea/io/fabric8/mq/mq-client/1.2.0.redhat-163/)
then put it in the `<JBoss A-MQ Home>/extras` directory. 

You can rename it to `mq-client.jar` to make it simpler.

To run it we will need another Terminal window. Executing `java -jar mq-client.jar` will have it list its options.

In messaging systems there are two (2) types of clients:
• Producers – who sending messages
• Consumers – who receive messages 

So let's continue...

Run the two (2) following command in your Terminal. The only difference between the two (2) is the first argument: producer or consumer. If you try to run this in a single Terminal, you need to run the producer version first, as the consumer will wait (block) for messages. You will ultimately want to have, at least, two Terminal windows going, so you might as well do that now...

```
java -jar mq-client.jar producer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 10
```

Run Consumer:

```
java -jar mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 10
```

The quick version of what the parameters are is:
  * `brokerUrl` – broker connection string
  * `user` / `password` – authentication credentials (i.e. must match etc/users.properties settings)
  * `destination` – JMS destination name – (queue|topic)://<name>
  * `count` – number of messages to send (producer) or wait for (consumer)

The connection string uses the Apache ActiveMQ failover transport (`failover:// `prefix) that has the client automatically re-connect when the connection is lost. It is followed by `tcp://<hostname>:<port>`.


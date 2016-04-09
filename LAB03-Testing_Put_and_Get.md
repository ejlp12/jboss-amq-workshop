# LAB 03 – TESTING JBOSS A-MQ FOR THE FIRST TIME

JBoss A-MQ includes a messaging test client as part of its distribution that helps you validate that everything is working correctly. 

1. Download client application

   You can download the client jar library (`mq-client-1.2.0.redhat-630045.jar`) from [this site](https://repository.jboss.org/nexus/content/repositories/ea/io/fabric8/mq/mq-client/1.2.0.redhat-630045/mq-client-1.2.0.redhat-630045.jar) then put it in the `<JBoss A-MQ Home>/extras` directory. 
   
   > If you did not find the above file, try other version by browsing to this link https://repository.jboss.org/nexus/content/repositories/ea/io/fabric8/mq/mq-client/

   You can rename it to `mq-client.jar` to make it simpler.

2. Run application

   To run it we will need another Terminal window. Executing `java -jar mq-client.jar` will have it list its options.

   In messaging systems there are two (2) types of clients:
   * __Producers__ – who sending messages
   * __Consumers__ – who receive messages 

   So let's continue...

   Run the two (2) following command in your Terminal. The only difference between the two (2) is the first argument: `producer` or `consumer`. 

   If you try to run this in a single Terminal, you need to run the producer version first, as the consumer will wait (block) for messages. You will ultimately want to have, at least, two Terminal windows going, so you might as well do that now...

   Run Producer:

   ```
   java -jar mq-client.jar producer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 10
   ```

   You will see that Producer is producing 10 messages and put it into queue with name `summit.test` in the A-MQ.
   
   ![image](https://cloud.githubusercontent.com/assets/3068071/12399318/d9adac12-be4b-11e5-935b-668dc7bcdc5e.png)
   
   Run Consumer:
   
   ```
   java -jar mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 10
   ```
   You will see that Producer is getting 10 messages from the same queue name in the A-MQ.
   
   ![image](https://cloud.githubusercontent.com/assets/3068071/12399322/e2132efe-be4b-11e5-8762-c001df57f0f3.png)

   The quick version of what the parameters are is:
     * `brokerUrl` – broker connection string
     * `user` / `password` – authentication credentials (i.e. must match etc/users.properties settings)
     * `destination` – JMS destination name – (queue|topic)://<name>
     * `count` – number of messages to send (producer) or wait for (consumer)

   The connection string uses the Apache ActiveMQ failover transport (`failover:// `prefix) that has the client automatically re-connect when the connection is lost. It is followed by `tcp://<hostname>:<port>`.


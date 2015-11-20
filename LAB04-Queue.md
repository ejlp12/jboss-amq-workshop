# LAB 04 – QUEUES

JBoss A-MQ is a fully compliant JMS 1.1 broker, which means it supports things like JMS Queues. Queues are for point-to-point messaging, that is one and only one consumer will receive any given message. We saw an example of queues in the last lab where we tested a producer and a consumer client against JBoss A-MQ. So now lets see what happens if we have two (2) consumers running...

For this lab, best to have at least four (4) Terminal windows open; Note order of execution is import, so please do the steps below in order

1. Terminal 1 (A-MQ) – Running JBoss A-MQ – see Lab 01 – Starting the JBoss A-MQ server if you need to re-start A-MQ

2. Terminal 2 (Consumer1)

    ```
    java -jar mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 50
    ```

3. Terminal 3 (Consumer2) 

    ```
    java -jar mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 50
    ```

4. Terminal 4 (Producer)

    ```
    java -jar mq-client.jar producer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination queue://summit.test --count 100 --sleep 100
    ```

You should see results like the following

**[IMAGE]**

Notice how in the Consumers, the messages are interleaving, in this case Consumer1 is getting all the odd numbered messages, and Consumer2 is getting all the even numbered message. In messaging this is called Competing Consumers. JBoss A-MQ will, by default, round robin load balance message dispatch across all the consumers on a Queue.

Try some different combinations for producers and consumers on your own. The most important part is ensuring they are all connecting to the same `--destination` (the JMS specification calls Queues and Topics generically Destinations). You can also increase the delay between message send (`--sleep`) in the producer if you'd like to try starting a mid-way...

Also try running the producer only, and then the consumer only. You should see A-MQ store the messages in-between. This is a key benefit of JMS Queues in that they allow producers and consumers to communication asynchronously, meaning they don't both have to be running at the same time to receive messages. This can be a big benefit to many systems designs where one system can be done for maintenance, while other systems can continue sending and receiving messages...

# LAB 05 – PUBLISH/SUBSCRIBE (TOPICS)

JBoss A-MQ is a fully compliant JMS 1.1 broker, which means it also supports things like __JMS Topics__. Topics are for publishing messages to all subscribers on a names topic. This is different from Queues, as Queues are 1-to-1 and Topics are 1-to-Many, that is all subscribers to a Topic get a copy of the message.

For this lab, best to have at least four (4) Terminal windows open; Note order of execution is import so please do the steps below in order

1. Terminal 1 (A-MQ) – Running JBoss A-MQ – see Lab 02 – Starting the JBoss A-MQ server if you need to re-start A-MQ

2. Terminal 2 (Consumer1) 

    ```
    java -jar mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination topic://summit.test --count 10
    ```

3. Terminal 3 (Consumer2) 

    ```
    java -jar mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination topic://summit.test --count 10
    ```

4. Terminal 4 (Producer) 

    ```
    java -jar mq-client.jar producer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination topic://summit.test --count 10
    ```

You should see results like the following

**Producer**
```
$java -jar extras/mq-client.jar producer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination topic://workshop.test --count 10        
Using destination: topic://workshop.test, on broker: failover://tcp://localhost:61616
[org.apache.activemq.transport.failover.FailoverTransport] : Successfully connected to tcp://localhost:61616
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
[io.fabric8.mq.ProducerThread] : Thread-0 Elapsed time in milli second : 11 milli seconds
[io.fabric8.mq.ActiveMQService] : Closed JMS connection
[io.fabric8.mq.ProducerThread] : Thread-0 Producer thread finished
All threads completed their work
```

**Consumer1**
```
$java -jar extras/mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination topic://workshop.test --count 10
Using destination: topic://workshop.test, on broker: failover://tcp://localhost:61616
[org.apache.activemq.transport.failover.FailoverTransport] : Successfully connected to tcp://localhost:61616
[io.fabric8.mq.ConsumerThread] : Thread-2 Waiting for: 10 messages
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 0
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 1
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 2
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 3
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 4
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 5
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 6
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 7
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 8
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 9
Thread-2 Consumed: 10 messages
[io.fabric8.mq.ConsumerThread] : Thread-2 Consumer thread finished
All threads completed their work
```

**Consumer2**
```
$java -jar extras/mq-client.jar consumer --brokerUrl failover://tcp://localhost:61616 --user admin --password admin --destination topic://workshop.test --count 10
Using destination: topic://workshop.test, on broker: failover://tcp://localhost:61616
[org.apache.activemq.transport.failover.FailoverTransport] : Successfully connected to tcp://localhost:61616
[io.fabric8.mq.ConsumerThread] : Thread-2 Waiting for: 10 messages
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 0
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 1
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 2
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 3
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 4
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 5
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 6
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 7
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 8
[io.fabric8.mq.ConsumerThread] : Thread-2 Received test message: 9
Thread-2 Consumed: 10 messages
[io.fabric8.mq.ConsumerThread] : Thread-2 Consumer thread finished
All threads completed their work
```

Now this isn't a cut and paste duplicate, you'll see that both consumers received all of the messages sent by the producer.

With a Topic all connected consumers (subscribers) will receive all messages sent by the producers (publishers). The key part is “connected” consumers. **A normal Topic consumer only receives messages sent after that consumer connects**. This is another big difference from Queues, which will save messages.

Now experiment and try some different combinations. Remember you can add a delay between message sends (`--sleep <ms>`), and change the number of messages sent or waited for (`--count`).

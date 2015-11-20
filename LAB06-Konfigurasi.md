# LAB 06  – ACTIVEMQ CONFIGURATION

JBoss A-MQ embeds Apache ActiveMQ, which is providing the bulk of the reliable messaging capabilities. ActiveMQ's configuration is primarily driven by a configuration file located by default in `<JBoss A-MQ Home>/etc/activemq.xml`. This configuration file, combined with some property value substitution at load time, determine how the ActiveMQ server will behave.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:amq="http://activemq.apache.org/schema/core" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.0.xsd http://activemq.apache.org/schema/core http://activemq.apache.org/schema/core/activemq- core.xsd">
   <!-- Allows us to use system properties and fabric as variables in this configuration file -->
   <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
      <property name="properties">
         <bean class="org.fusesource.mq.fabric.ConfigurationProperties" />
      </property>
   </bean>
   <broker xmlns="http://activemq.apache.org/schema/core" brokerName="${broker-name}" dataDirectory="${data}" start="false">
      <destinationPolicy>
         <policyMap>
            <policyEntries>
               <policyEntry topic="&gt;" producerFlowControl="true">
                  <pendingMessageLimitStrategy>
                     <constantPendingMessageLimitStrategy limit="1000" />
                  </pendingMessageLimitStrategy>
               </policyEntry>
               <policyEntry queue="&gt;" producerFlowControl="true" memoryLimit="1mb" />
            </policyEntries>
         </policyMap>
      </destinationPolicy>
      <managementContext>
         <managementContext createConnector="false" />
      </managementContext>
      <persistenceAdapter>
         <kahaDB directory="${data}/kahadb" />
      </persistenceAdapter>
      <plugins>
         <jaasAuthenticationPlugin configuration="karaf" />
      </plugins>
      <systemUsage>
         <systemUsage>
            <memoryUsage>
               <memoryUsage limit="64 mb" />
            </memoryUsage>
            <storeUsage>
               <storeUsage limit="100 gb" />
            </storeUsage>
            <tempUsage>
               <tempUsage limit="50 gb" />
            </tempUsage>
         </systemUsage>
      </systemUsage>
      <transportConnectors>
         <transportConnector name="openwire" uri="tcp://0.0.0.0:0?maximumConnections=1000" />
      </transportConnectors>
   </broker>
</beans>
```

Here's a quick overview of the major components of the ActiveMQ configuration file. You can read more about its configuration here – https://access.redhat.com/site/documentation/JBoss_A-MQ/. 

First thing you may notice is that its a Spring file; ActiveMQ is configured using dependency injection.

* DestinationPolicy – These are configuration and constraint policies that are applied to Destinations (Queues and Topics) based on wildcards. In this file we're seeing policies to constrain any specific Queue to have no more than 1mb of messages in memory before it starts to stop producers from adding more messages to it.
* ManagementContext – This configures the extensive JMX support within ActiveMQ. In this file, its defaulting to using to parent containers JMX connection.
* PersistenceAdapter – This controls how and where Persistent messages are stored for delivery. By default, ActiveMQ stores messages into its persistence store before acknowledging its receipt to the producer – a reliable hand off. This allows ActiveMQ to survive a restart, and continue to deliver all persistent messages in its store. This file is using the KahaDB file based persistence store.
* JaasAuthenticationPlugin – JBoss A-MQ and ActiveMQ use the JAAS (Java Authentication and Authorization Standard) APIs for connection to identity and authorization stores. In this case, ActiveMQ is using the A-MQ containers setup, which by default uses the etc/users.properties file as a flat identity and role store (remember you edited this file back in Lab 01 – Installing JBoss A-MQ).
* SystemUsage – These are limits that ActiveMQ uses to constrain its use of resources (memory and disk) to what you've allocated to it. Its a really bad thing when ActiveMQ runs out of disk while trying to write a message to it. These settings allow ActiveMQ to gracefully stop accepting messages from producers when the limits are exceeded, and to restart after consumers have for example pulled enough messages from it.
* TransportConnectors – This is the list of one or more wire protocol handlers that ActiveMQ uses for messaging clients to connect to it. JBoss A-MQ supports many wire protocols such as AMQP 1.0, MQTT, STOMP, and OpenWire. It also includes variations like combining with SSL. This example is using OpenWire.

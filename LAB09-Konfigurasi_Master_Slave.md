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


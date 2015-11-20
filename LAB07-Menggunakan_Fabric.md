# LAB 07 – CONFIGURING FABRIC

JBoss A-MQ includes a technology called Fabric that can help with setting up and managing many instances of JBoss A-MQ (and JBoss Fuse). It provides a centralized configuration management and provisioning capability, and it includes a runtime registry that allows clients to auto-discovery where named groups of instances are (host, port, etc.).

In this lab, we're going to configure the first component of a Fabric deployment, the Fabric controller including the Fabric Management Console.

In the Terminal window that has JBoss A-MQ running (if you need to re-start, look at Lab 02 – Starting the JBoss A-MQ server), type the following command

```
       fabric:create --clean --zookeeper-password admin --profile fmc
```

You will see the Console restart (it will re-display the JBoss A-MQ ASCII art). At this point, this instance of JBoss A-MQ has performed a quick change act under the covers.


* It has shutdown its embedded ActiveMQ instance (try one of the messaging clients now, it should not be able to connect).
* It has started a Fabric Registry (run `fabric:status` and look for the `fabric-ensemble-xxx` entry).
* It has added new commands. Hit `<tab>` and it will now ask if you want to see all 354 possibilities. Type `fabric:<tab>` and you will see many more options than before.
* And it has started the Fabric Management Console (Web GUI for Fabric also known as FMC). We'll look at FMC in a later lab.

That's a lot for one little command...

Before I spend a little time explaining what just happened, this would be a good time to stand and say “I am now the master of my own Fabric!”. You're entering a brave new world, don't look back...

The `fabric:create` command had a few components


* `--clean` – resets your container back to its install state so that if you've installed or uninstalled anything, we'll wipe that away so you're starting in a known state with your new management Fabric.
* `--zookeeper-password <password>` – this is the password token used to allow other container instances to join your Fabric. So if you're friend on the next lab machine wanted to join your Fabric (you know he's always looked up to you), then they could type `fabric:join – zookeeper-password <password> <your host>:2181`. The last bit is called the “zookeeperURL”.
* `--profile fmc` – Fabric uses a concept called Profiles, which are a combination of a set of code bits to deploy, and configuration property values for those code bits. Its very easy to create, and version, Profiles in Fabric. This makes it much easier to manage many node deployments. In this case, we told this container to switch to the `fmc` profile when it became the Fabric Controller.

Enough with the explanations, for now. Let's go to the next lab, and explore the FMC web GUI...

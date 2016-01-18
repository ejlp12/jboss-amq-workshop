# LAB 07 – CONFIGURING FABRIC

JBoss A-MQ includes a technology called Fabric that can help with setting up and managing many instances of JBoss A-MQ (and JBoss Fuse). It provides a centralized configuration management and provisioning capability, and it includes a runtime registry that allows clients to auto-discovery where named groups of instances are (host, port, etc.).

In this lab, we're going to configure the first component of a Fabric deployment, the Fabric controller including the Fabric Management Console.

1. Start and create a Fabric

   In the Terminal window that has JBoss A-MQ running (if you need to re-start, look at Lab 02 – Starting the JBoss A-MQ server), type the following command:

   ```
   fabric:create --clean --wait-for-provisioning 
   ```

   The container will re-provision itself. You should see the following messages print to the Console:

   ```
   Waiting for container: root
   Waiting for container root to provision.
   ```
   
   You might also see additional line `Error executing command: Unable to create zookeeper server configuration`

   Once the re-provisioning operation has completed, you will be returned to the prompt. At this point, this instance of JBoss A-MQ has performed a quick change act under the covers.
   
    * It has shutdown its embedded ActiveMQ instance (try one of the messaging clients now, it should not be able to connect).
    * It has started a Fabric Registry (run `fabric:status` and look for the `fabric-ensemble-xxx` entry).
    * It has added new commands. Hit `<tab>` and it will now ask if you want to see all 354 possibilities. Type `fabric:<tab>` and you will see many more options than before.
    * And it has started the Fabric Management Console (Web GUI for Fabric also known as FMC). We'll look at FMC in a later lab.
    
  That's a lot for one little command...

  Before I spend a little time explaining what just happened, this would be a good time to stand and say “I am now the master of my own Fabric!”. You're entering a brave new world, don't look back...

  The `fabric:create` command had a few components

    * `--clean` – resets your container back to its install state so that if you've installed or uninstalled anything, we'll wipe that away so you're starting in a known state with your new management Fabric.
    * `--wait-for-provisioning` - This just tells the Console to block until all of the required bundles and resources are available, installed, and running for fabric. 
   
   
2. Try some fabric command

   ```
   JBossA-MQ:karaf@root> fabric:container-list
   [id]   [version]  [type]  [connected]  [profiles]              [provision status]
   root*  1.0        karaf   yes          fabric                  success
                                          fabric-ensemble-0000-1
                                          mq-amq
   ```
   
   ```
   JBossA-MQ:karaf@root> fabric:cluster-list
    [cluster]                                                               [masters]  [slaves]  [services]                                      []
    amq/default
       root                                                                 root       -         tcp://ejlp-macbook:61617
    apis/rest/FabricResource/fabric8/1.0
       root                                                                 root       -         http://ejlp-macbook:8181/cxf/fabric8
    git
       fabric-repo                                                          root       -         http://ejlp-macbook:8181/git/fabric/
    servlets/io.fabric8.fabric-git-server/1.2.0.redhat-133/git
       root                                                                 root       -         http://ejlp-macbook:8181/git
    servlets/io.fabric8.fabric-jolokia/1.2.0.redhat-133/jolokia
       root                                                                 root       -         http://ejlp-macbook:8181/jolokia
    servlets/io.fabric8.fabric-maven-proxy/1.2.0.redhat-133/maven/download
       root                                                                 root       -         http://ejlp-macbook:8181/maven/download
    servlets/io.fabric8.fabric-maven-proxy/1.2.0.redhat-133/maven/upload
       root                                                                 root       -         http://ejlp-macbook:8181/maven/upload
    servlets/io.fabric8.fabric-redirect/1.2.0.redhat-133/*
       root                                                                 root       -         http://ejlp-macbook:8181/*
    servlets/io.fabric8.mq.mq-http-discovery/1.2.0.redhat-133/mq-discovery
       root                                                                 root       -         http://ejlp-macbook:8181/mq-discovery
    servlets/org.apache.cxf.cxf-rt-transports-http/3.0.4.redhat-620133/cxf
       root                                                                 root       -         http://ejlp-macbook:8181/cxf
    webapps/io.hawt.hawtio-karaf-terminal/1.4.0.redhat-133
       root                                                                 root       -         http://ejlp-macbook:8181/hawtio-karaf-terminal
    webapps/io.hawt.hawtio-redhat-fuse-branding/1.4.0.redhat-133
       root                                                                 root       -         http://ejlp-macbook:8181/redhat-branding
    webapps/io.hawt.hawtio-web/1.4.0.redhat-133
       root                                                                 root       -         http://ejlp-macbook:8181/hawtio
    webapps/io.hawt.swagger.hawtio-swagger-ui/1.4.0.redhat-133
       root                                                                 root       -         http://ejlp-macbook:8181/hawtio-swagger
    JBossA-MQ:karaf@root> fabric:cluster-list --help
    ```

   Enough with playing some command for now. Let's go to the next lab, and explore the FMC web GUI...

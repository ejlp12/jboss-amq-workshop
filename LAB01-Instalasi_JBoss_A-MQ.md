# LAB 01 – INSTALLING JBOSS A-MQ

If you are starting from scratch, and not on a lab machine, you can continue this lab section with following instructions.

Installing JBoss A-MQ 6.2 is easy. You'll need to unzip the distribution into the target directory. You can get a JBoss A-MQ distribution from http://www.jboss.org/products/amq.


![image](https://cloud.githubusercontent.com/assets/3068071/11288482/030d58ec-8f57-11e5-976b-fa4dfea625cf.png)

If you have Subscription, then you can download from Red Hat Customer Portal:
https://access.redhat.com/jbossnetwork/restricted/listSoftware.html?product=jboss.amq&downloadType=distributions&version=6.2.0

![image](https://cloud.githubusercontent.com/assets/3068071/11288377/00b6dac4-8f56-11e5-903c-193a0a4ef421.png)

1. Extract (unzip) JBoss A-MQ binary installer.

   ```
   $ mkdir Servers
   $ cd Servers
   $ unzip ~/Downloads/jboss-a-mq-6.2.0.redhat-133.zip
   ```
   This will create a subdirectory named `jboss-a-mq-6.2.0.redhat-133`.

2. Edit configuration file

   You will need to edit a couple of configuration files after your first install to setup an administrative username and password.

   Edit `<JBoss A-MQ Home>/etc/users.properties`, and uncomment (remove the leading `#`) the last line. The format is `<username>=<password>,<role>,<role>`. For the purposes of this lab, let's use `admin` as a username and password is `admin`.

   ```
   admin=admin,admin
   ```

3. Start JBoss A-MQ

   Go to `<JBoss A-MQ Home>/bin` directory and Start the A-MQ broker using following command

   ```
   cd /Servers/jboss-a-mq-6.2.0.redhat-133/bin
   ./amq
   ```
   You will get following output and it will prompt you with A-MQ console:

    ```
    Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=128M; support was removed in 8.0
    Please wait, JBoss A-MQ is initializing...
    100% [========================================================================]
    
          _ ____                                __  __  ____
         | |  _ \                    /\        |  \/  |/ __ \
         | | |_) | ___  ___ ___     /  \ ______| \  / | |  | |
     _   | |  _ < / _ \/ __/ __|   / /\ \______| |\/| | |  | |
    | |__| | |_) | (_) \__ \__ \  / ____ \     | |  | | |__| |
     \____/|____/ \___/|___/___/ /_/    \_\    |_|  |_|\___\_\
    
      JBoss A-MQ (6.2.0.redhat-133)
      http://www.redhat.com/products/jbossenterprisemiddleware/amq/
    
    Hit '<tab>' for a list of available commands
    and '[cmd] --help' for help on a specific command.
    
    Open a browser to http://localhost:8181 to access the management console
    
    Hit '<ctrl-d>' or 'osgi:shutdown' to shutdown JBoss A-MQ.
    
    JBossA-MQ:karaf@root>
    ```

   That's it! An install so fast, you don't even have time to get a cup of coffee...


By default, JBoss A-MQ will start in **interactive mode**, which means it both starts the server, and displays the command Console. There are startup options to refine this, such as 

* `./bin/amq server` to just start the server without the Console, and... 
* `./bin/amq client` to start just the Console that will connect to a locally running server.

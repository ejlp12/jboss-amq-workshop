# LAB 08 – EXPLORING FABRIC MANAGEMENT CONSOLE

Now that you are master of your own Fabric (if you aren't, follow the steps in Lab 08 – Configuring Fabric), we should also have at our disposal a web GUI. Open your local web browser and go to http://localhost:8181 and you should see something like this...

[IMAGE]

The login information is what you configured in Lab 01 – Installing JBoss A-MQ (hint: admin/admin). Go ahead and Log In...

There's a lot here, so let me give you the quick summary of the top tab bar so you can explore on your own.

* Containers – list of all instances (local and remote) that are joined to this Fabric
* Cloud – let's you start and manages managed containers in the Cloud (right now just Amazon EC2)
* Profiles – list of all Profiles (code bits to deploy with configuration values), and versions of Profiles
* Patching – Fabric supports the ability to patch its managed containers
* Users – Quick GUI to manage users configured in the `etc/users.properties` file. Your
Production deployment will probably have JBoss A-MQ configured to use your LDAP or Active Directory environment, and you'd use its tools to manage those user stores...

So go explore on your own for a bit, and then in the next lab we'll walk you though a couple of multiple container configurations. If you manage to really break your environment (perhaps having too much fun clicking buttons, eh?), then you can go back to Lab 07 – Configuring Fabric and clean up before the next lab.

[IMAGE]

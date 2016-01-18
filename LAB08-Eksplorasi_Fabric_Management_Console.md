# LAB 08 – EXPLORING FABRIC MANAGEMENT CONSOLE

Now that you are master of your own Fabric (if you aren't, follow the steps in Lab 07 – Configuring Fabric), we should also have at our disposal a web GUI. Open your local web browser and go to [http://localhost:8181](http://localhost:8181) and you should see something like this:


![image](https://cloud.githubusercontent.com/assets/3068071/12400572/8281e9d2-be53-11e5-9300-71fa99e026a8.png)

The login information is what you configured in Lab 01 – Installing JBoss A-MQ (hint: admin/admin). Go ahead and Log In...

Once you’ve logged in, you will be greeted with a welcome screen. Click “Containers” to view your list of containers.

![image](https://cloud.githubusercontent.com/assets/3068071/12400648/f3b0d03c-be53-11e5-9248-0ecc49e5df86.png)

There's a lot here, so let me give you the quick summary of the top tab bar so you can explore on your own.

* Containers - List of all instances (local and remote) that are joined to this Fabric
* Profiles - List of all Profiles (code bits to deploy with configuration values), and versions of Profiles
* Wiki - List of all Profiles in wiki format (with README.md files properly displayed)
* Services - List of containers, MQ brokers, EIP endpoints, API endpoints, and Fabric registry hierarchy
* Scaling - View of application/profile scaling requirements
* Dashboard - Custom dashboards for monitoring
* Health - Overall health of the system


So go explore on your own for a bit, and then in the next lab we'll walk you though a couple of multiple container configurations. If you manage to really break your environment (perhaps having too much fun clicking buttons, eh?), then you can go back to Lab 07 – Configuring Fabric and clean up before the next lab.


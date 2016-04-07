# LAB 02 – INTERACTING WITH JBOSS A-MQ CONSOLE
Now that we have a running local JBoss A-MQ server, you have a whole new command Console to play with...

The JBoss A-MQ container supports an SSHD server that allows you to SSH into your running server to manage it. When you start up JBoss A-MQ, by default, it displays the Console for the instance you just started. You can also use a script to connect to other local and remote instances (check out `<JBoss A-MQ Home>/bin/client --help`). 

This Console supports hundreds of commands to manage your JBoss A- MQ server. The console also supports limited scripting (think simple BASH shell scripts), and command output pipelining (`log:display |grep started`) – try that in your father's server...

Explore the Shell on your own for a little bit. Hit <tab> to display a list of commands (281 and counting). To see the help for any command type its name followed by `--help`.

Try to give the console following command:

```
fabric:welcome  - Prints the Fabric welcome message
info            - Prints system information
list            - Lists all installed bundles.
ls              - Lists OSGi services.
features:list   - Lists all existing features available from the defined repositories.
web:list        - Lists details for war bundles.
log:display     - Displays log entries.
log:tail        - Continuously display log entries. Use ctrl-c to quit this command
```

You also try following command:

```
features:list | more
```


When you're done exploring, move on to the next lab...

<strong>VNC Server Setup</strong>

How to setup VNC Server on a CentOS 7 server.

#### Install the server package


~~~~
 yum install -y tightvnc-server

 yum install -y xterm</code>
~~~~


#### Copy the config file


~~~~
 cp /lib/systemd/system/vncserver\@.service /etc/systemd/system/vncserver@:1.service
~~~~


#### Setup the service for users



Edit the config file adding the username of the user going to run the server session.

Add the -localhost option to prevent VNC being available over the network (insecure)


~~~~

[Service]
Type=forking
# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i &gt; /dev/null 2&gt;&amp;1 || :'
ExecStart=/sbin/runuser -l -c "/usr/bin/vncserver -localhost %i"
PIDFile=/home//.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i &gt; /dev/null 2&gt;&amp;1 || :'
~~~~


#### Enable the service


~~~~
 systemctl daemon-reload</code>

 systemctl enable vncserver@:1.service
~~~~


To manually run the service as non-root user and not using systemd:

`# vncserver`

The first time it runs it prompts you for a password. They are stored in ~/.vnc/passwd


Stop the manual run of the service with:

`# vncserver -kill :1`


On windows client:

Download and install tight vnc. You only need client, not server package.

Configure putty to forward X11 traffic to your windows host.

X11 Select options "Enable X11 forwarding" "X display location: localhost:0"
Tunnels "Add new forwarded port: Source port: 5900" "Destination: :5901"

Finally, start up the client and connect VNC viewer to localhost:5901

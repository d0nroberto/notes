# Creating and Modifying systemd Unit Files

A unit file contains configuration details for a service etc. 
Several systemctl commands work with unit files in the background. 

Unit Types are:

|Unit Type|Description|
|---------|-----------|
|service|System service|
|target|A group of systemd units|
|automount|Automount mount point|
|device|A device file|
|mount|A file system mount point|
|path|A file or directory in a file system|
|scope|An externally created process|
|slice|A group of hierarchically organized units that manage system processes|
|snapshot|A saved state of the systemd manager|
|socket|An IPC socket|
|swap|A swap device / file|
|time| A systemd timer.|


Files are named with the following format: <unit_name>.<type> 
examples include: rsyncd.service, rsyncd.socket

To make adjustments, system administrator must edit or create unit files manually. 


|Directory|Description|
|---------|-----------|
|/usr/lib/systemd/system/|Systemd units distributed with installed RPM packages|
|/run/systemd/system/|Systemd units created at run time. This directory takes precedence over the directory with installed service units|
|/etc/systemd/system/|Systemd units created and managed by the system administrator. This directory takes precedence over the directory with runtime units|

##### Do not modify any files under /usr/lib/systemd/system.

The /etc/systemd/system/ directory is reserved for unit files created or modified by the system administrator

Existing units can be modified without changing the base file.

Either copy the existing unit file from /usr/lib/systemd/system/ to /etc/systemd/system/ and edit the file there or create a custom folder for the unit and add your own configuration there e.g. /etc/systemd/system/rsyncd.servicd.d/custom_config.conf

To apply changes to unit files without a reboot: systemctl daemon-reload
You then need to restart the service: systemctl restart rsyncd.service

e.g.
~~~~
# mkdir /etc/systemd/system/rsyncd.service.d
# vim /etc/systemd/system/rsyncd.service.d/rsync_customisations.conf
~~~~


Unit file has the following format.

There are 3 sections

~~~~
[Unit] - general options

[unit type] - Type specific directives e.g. [Service] options

[Install] - Options used when systemctl enable / disable command is used
~~~~

Options: see the manpage for more details.


### [Unit] Section options

*Description* Displayed for example in the output of the systemctl status command.  
*Documentation* Documentation URLs.  
*After* Defines the order in which units are started. The unit starts only after the units specified in After are active.  
*Before* Defines the order in which units are started. The unit starts only before the units specified in Before are active  
*Requires* Configures dependencies on other units. The units listed in Requires are activated together with the unit. If any of the required units fail to start, the unit is not activated.  
*Conflicts* Configures negative dependencies, an opposite to Requires.  
*Wants* Configures weaker dependencies than Requires. If any of the listed units does not start successfully, it has no impact on the unit activation.  


### [Service] Unit Type Section options

*Type* Startup Type. simple / forking / oneshot / dbus / notify / idle  
*ExecStart* Specifys command / script to run at startup. Also look at ExecStartPre & ExecStartPost  
*ExecStop* Specifys command / script to run at stop.  
*ExecReload* Specifys command / script to run at reload.  
*Restart* Service restarts when process exits and is not from a clean systemctl stop  
*RemainAfterExit* If True the service is considered active even if the process exits.  


### [Install] Section Options

*Alias* Space separated list of alternate unit names  
*RequiredBy* Units that depend on this unit. They gain a Require dependency when it is enabled  
*WantedBy* Units that weakly depend on this unit. They gain a Want dependency when it is enabled  
*Also* List of units to be installed / uninstalled with this unit.  


e.g. rsyncd.service:

~~~~
[Unit]
Description=fast remote file copy program daemon
ConditionPathExists=/etc/rsyncd.conf

[Service]
EnvironmentFile=/etc/sysconfig/rsyncd
ExecStart=/usr/bin/rsync --daemon --no-detach "$OPTIONS"

[Install]
WantedBy=multi-user.target
~~~~

e.g. vncserver.service:

~~~~
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target

[Service]
Type=forking
# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
ExecStart=/usr/sbin/runuser -l <USER> -c "/usr/bin/vncserver -localhost %i"
PIDFile=/home/<USER>/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

[Install]
WantedBy=multi-user.target
~~~~


e.g. httpd.service:

~~~~
[Unit]
Description=The Apache HTTP Server
After=network.target remote-fs.target nss-lookup.target
Documentation=man:httpd(8)
Documentation=man:apachectl(8)

[Service]
Type=notify
EnvironmentFile=/etc/sysconfig/httpd
ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND
ExecReload=/usr/sbin/httpd $OPTIONS -k graceful
ExecStop=/bin/kill -WINCH ${MAINPID}
# We want systemd to give httpd some time to finish gracefully, but still want
# it to kill httpd after TimeoutStopSec if something went wrong during the
# graceful stop. Normally, Systemd sends SIGTERM signal right after the
# ExecStop, which would kill httpd. We are sending useless SIGCONT here to give
# httpd time to finish.
KillSignal=SIGCONT
PrivateTmp=true

[Install]
WantedBy=multi-user.target
~~~~

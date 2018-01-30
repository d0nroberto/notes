# Solaris Zones

Solaris uses zones to implement the container principle you might be familiar with in docker.
The container will run a selection of processes without needing a full OS stack.
You can then run an instance of your application inside the zone.

The parent host is known as the global zone, while the container instances are known as local zones.

You use the *zonecfg* and *zoneadm* commands to create, configure and manage zones.

The local zone configuration is saved in xml files located in */etc/zones*. The xml filename is the same as the zone name.

Lets assume we want to create a zone - zoneA - running on global zone hostA.

Run the following zonecfg command & options to create an XML file.

~~~~
# zonecfg -z zoneA
zoneA: No such zone configured
Use 'create' to begin configuring a new zone.
zonecfg:zoneA> create
zonecfg:zoneA> set zonepath=/export/zones/zoneA
zonecfg:zoneA> set autoboot=true
zonecfg:zoneA> add net
zonecfg:zoneA:net> set address=192.168.179.22/24
zonecfg:zoneA:net> set physical=e1000g0
zonecfg:zoneA:net> end
zonecfg:zoneA> exit
~~~~

Where *physical=e1000g0* represents the NIC used by the host.

To see the network cards available to you, use the dladm command:

~~~~
# dladm show-dev
e1000g0         link: up        speed: 1000  Mbps       duplex: full
~~~~

You will also need to make sure the */export/zones* folder exists.

Now we will install the zone, based on the configuration we created.

~~~~
# zoneadm -z zoneA install
Preparing to install zone <zoneA>.
Creating list of files to copy from the global zone.
Copying <58990> files to the zone.
Initializing zone product registry.
Determining zone package initialization order.
Preparing to initialize <1566> packages on the zone.
Initializing package <555> of <1566>: percent complete: 35%
...
The file </export/zones/zoneA/root/var/sadm/system/logs/install_log> contains a log of the zone installation.
~~~~

Note the zone is not yet running, but is 'installed'.

~~~~
# zoneadm list -iv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
~~~~

You will now boot the zone, again using the zoneadm command:

~~~~
# zoneadm -z zoneA boot
# zoneadm list -iv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   1 zoneA            running    /export/zones/zoneA            native   shared
~~~~

To access the zone console, use the zlogin command:

~~~~
# zlogin -C zoneA
[Connected to zone 'zoneA' console]
~~~~

On initial login you may get asked for info to complete sysidtool e.g. language, default terminal.

Just follow through the remaining prompts of the installer and the zone will reboot e.g.

~~~~
Select a Language


  0. English
  1. es
  2. fr


Please make a choice (0 - 2), or press h or ? for help: 0
...
[NOTICE: Zone rebooting]

SunOS Release 5.10 Version Generic_150401-07 64-bit
Copyright (c) 1983, 2013, Oracle and/or its affiliates. All rights reserved.
Hostname: zoneA
~~~~

To log out of your console session hot the following keys in quick succession: 
~~~~
~.
~~~~

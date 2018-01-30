### Cloning Solaris Zone Containers

Lets assume we have a zone - zoneA - running on global zone hostA.
What we will do here is clone zoneA to a new zoneB running on the same host.

First lets stop the running zoneA & clone it's configuration to zoneB:

~~~~
# zoneadm -z zoneA halt
# zonecfg -z zoneA export -f /tmp/zoneB.cfg
~~~~

Edit the zone config to change any parameters you need to update e.g. IP / NIC / zonepath

~~~~
# vim /tmp/zoneB.cfg
~~~~

And now import the zone config, then clone the original:

~~~~
# zonecfg -z zoneB -f /tmp/zoneB.cfg

# zoneadm list -cv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
   - zoneB            configured /export/zones/zoneB            native   shared

# zoneadm -z zoneB clone zoneA

# rm /tmp/zoneB.cfg
~~~~

You should now have your new installed:

~~~~
# zoneadm list -cv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
   - zoneB            installed  /export/zones/zoneB            native   shared
~~~~   

And boot everything back up!

~~~~
# zoneadm -z zoneA boot

# zoneadm -z zoneB boot

# zoneadm list -iv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   3 zoneA            running    /export/zones/zoneA            native   shared
   4 zoneB            running    /export/zones/zoneB            native   shared
~~~~

You can now access the new zone using the zlogin command:

~~~~
# zlogin -C zoneB
[Connected to zone 'zoneB' console]
~~~~

This method re-runs the sysidconfig step to confirm the zone name, IP address etc.
If you are building a large number of zones at once, this adds a manual step.
An alternative method to using the zoneadm ... clone is to use ZFS snapshots.

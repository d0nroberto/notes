# Cloning Solaris Containers using ZFS

You can use the *zoneadm* command to carry out zone cloning.
You can similarly clone a zone using ZFS snapshots etc.

Lets  assume we have a zone - zoneA - running on global zone hostA that we wish to clone to a new zoneB.
Instead of running zoneadm commands, we will use the zfs commands to create a complete clone of the filesystems used by the new zone.

This assumes you have the zone zoneA contained on a single zfs mount like so:

~~~~
NAME                            USED  AVAIL  REFER  MOUNTPOINT
...
export/zones                   7.89G  1.79T    27K  /export/zones
export/zones/zoneA             3.95G  1.79T  3.95G  /export/zones/zoneA
...
~~~~

and the permissions on the zfs mount are not group / world read & write

~~~~
# ls -l /export/zones
total 6
drwx------   4 root     root           4 Nov 17 09:52 zoneA/
~~~~

First lets stop the running zoneA & clone it's configuration to zoneB:

~~~~
# zoneadm -z zoneA halt

# zonecfg -z zoneA export -f /tmp/zoneB.cfg
~~~~

Edit the zone config to change any parameters you need to update e.g. IP / NIC / zonepath

~~~~
# vim /tmp/zoneB.cfg
~~~~

And now import the zone config, then create the new ZFS zonepath for zoneB:

~~~~
# zonecfg -z zoneB -f /tmp/zoneB.cfg

# zoneadm list -cv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
   - zoneB            configured /export/zones/zoneB            native   shared

# zfs snapshot export/zones/zoneA@snap

# zfs send export/zones/zoneA@snap | zfs recv export/zones/zoneB

# zfs destroy export/zones/zoneA@snap

# zfs destroy export/zones/zoneB@snap

# rm /tmp/zoneB.cfg
~~~~

~~~~
# zoneadm list -cv
  ID NAME             STATUS     PATH                           BRAND    IP
   0 global           running    /                              native   shared
   - zoneA            installed  /export/zones/zoneA            native   shared
   - zoneB            installed  /export/zones/zoneB            native   shared
~~~~

You now need to edit the zoneB system files to reflect the new hostname, IP address etc.


~~~~
# vim /export/zones/zoneB/root/etc/inet/hosts

# vim /export/zones/zoneB/root/etc/nodename
~~~~

And also create the sysidconfig file that will be used to build out the system host info.

~~~~
# vim /export/zones/zoneB/root/etc/sysidcfg
~~~~

e.g.

~~~~
system_locale=C
timezone=Europe/Dublin
terminal=vt100
name_service=NONE
network_interface=PRIMARY {
  hostname=zoneB
  ip_address=192.168.179.69}
root_password=[password hash from /etc/shadow]
security_policy=NONE
nfs4_domain=dynamic
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

Note the zone may reboot once after running the sysidtool step.

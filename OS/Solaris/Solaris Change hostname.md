# Changing the hostname on a Solaris system


Follow these steps to change the hostname:

### run the sys-unconfig command

or

### Edit Files

/etc/nodename

/etc/hostname.*interface

/etc/inet/hosts

/etc/inet/ipnodes   # Starting with the Solaris 10 OS

~~~~
# find /etc -type f | xargs grep -l `hostname`
~~~~

Rename the host name directory within the /var/crash directory.

~~~~
# cd /var/crash
# mv old-host-name new-host-name
~~~~

Reboot:

~~~~
#init 6
~~~~

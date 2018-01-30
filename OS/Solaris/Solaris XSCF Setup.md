# Setting Up an XSCF card for a Solaris M series server

Some information in setting up a user account for the XSCF and accessing it over the network.
If you would prefer to [RTFM](https://docs.oracle.com/cd/E19580-01/821-2797-10/821-2797-10.pdf)...
Also this [info](https://docs.oracle.com/cd/E19415-01/E21618-01/index.html) is useful.


### Connect to XSCF (serial) and log in.

To configure an XSCF first use the XSCF "default" user account. 

~~~~
Default user account: default
The user privileges are useradm, platadm.
 Default password:
~~~~

The default password is not input directly on the keyboard. 
Instead, after the default user account is input, the mode key of the server front panel is operated as follows.

If Locked:
~~~~
 -> Change to Service -> Press return -> Keep the status for more than 5 seconds. 
 
 -> Change to Locked -> Press return
~~~~


Or if Service:
~~~~
 -> Change to Locked -> Press return -> Keep the status for more than 5 seconds. 
 
 -> Change to Service -> Press return
~~~~

This key turning has to be done within one minute, otherwise your session will timeout.


### Create a user account to access the XSCF.

From the XSCF console prompt *XSCF>*
~~~~
XSCF> adduser [username]
XSCF> password [password]
XSCF> setprivileges [username] [privs]
~~~~

These privileges should give you full admin rights on the XSCF: useradm platadm auditadm fieldeng, so...

~~~~
XSCF> setprivileges systemadmin useradm platadm auditadm fieldeng
~~~~

You can find the full privileges table [here](https://docs.oracle.com/cd/E19415-01/E21618-01/AccessControl.html#50491449_63170).


### Setup XSCF network access.

View the current network settings with

~~~~
XSCF> shownetwork -a
~~~~

To configure the network access, use the following commands:

~~~~
XSCF> setnetwork xscf#0-lan#0 -m [netmask] [ip address]
XSCF> setnetwork xscf#0-lan#1 -c down

XSCF> showhostname -a
xscf#0:localhost.localdomain
XSCF> sethostname xscf#0 [hostname]
XSCF> sethostname -d [domainname]

XSCF> showroute -a
Kernel IP routing table
Destination Gateway Netmask Flags Interface

XSCF> setroute -c add -n [network] -m [netmask] -g [gateway] xscf#0-lan#0
XSCF> setroute -c add -n [network] -m [netmask] -g [gateway] xscf#0-lan#0

XSCF> shownameserver
---
XSCF> setnameserver [nameserver ip]

XSCF> applynetwork

The following network settings will be applied:

xscf#0 hostname :hostname-xscf

....

XSCF> rebootxscf</code>
~~~~

For example...

~~~~
XSCF> setnetwork xscf#0-lan#0 -m 255.255.255.0 192.168.179.32
XSCF> setnetwork xscf#0-lan#1 -c down
XSCF> sethostname xscf#0 hostname-xscf
XSCF> sethostname -d example.com
XSCF> setroute -c add -n 192.168.179.0 -m 255.255.255.0 -g 192.168.179.1 xscf#0-lan#0
XSCF> setroute -c add -n 0.0.0.0 -m 255.255.255.0 -g 192.168.179.1 xscf#0-lan#0
XSCF> setnameserver 192.168.179.253
XSCF> applynetwork
XSCF> rebootxscf
~~~~

The setroute command syntax:

~~~~
XSCF> setroute -c [add|del] -n address [-m address] [-g address] interface
~~~~

-c specifies whether to add or delete routing information  
-n address specifies the IP address to which routing information is forwarded  
-m address specifies the netmask address to which routing information is forwarded  
-g address specifies the gateway address, and interface specifies the network interface to be set 


### Setting up the DSCP

The DSCP network is used internally for communication between the XSCF and the domains.
It is not accessed by users etc.

~~~~
XSCF> setdscp -i 192.168.2.0 -m 255.255.255.0

XSCF> showdscp

DSCP Configuration:

Network: 192.168.2.0
Netmask: 255.255.255.0

Location Address
---------- ---------
XSCF 192.168.2.1
Domain #00 192.168.2.2
Domain #01 192.168.2.3
Domain #02 192.168.2.4
Domain #03 192.168.2.5
~~~~


### Misc. Settings

Set the default password policy, time etc.

~~~~
showpasswordpolicy
setpasswordpolicy

showtimezone
settimezone

showdate
setdate

showntp
setntp
~~~~


### Enable ssh and https access.

~~~~
XSCF> sethttps -c genserverkey
Enter passphrase:
Verifying - Enter passphrase:

XSCF> sethttps -c selfsign IE MN Limerick Example Company hostmaster@example.com
CA key and CA cert already exist. Do you still wish to update? [y|n] :y
Enter passphrase:
Verifying - Enter passphrase:

XSCF> sethttps -c enable
Continue? [y|n] :y
Please reset the XSCF by rebootxscf to apply the https settings.
XSCF> setssh -q -y -c enable
XSCF> rebootxscf
The XSCF will be reset. Continue? [y|n] :y
XSCF>
~~~~

To view the SSH key:

~~~~
XSCF> showssh -c pubkey
~~~~

To generate a key pair for ssh:

~~~~
XSCF> setssh -c genhostkey
~~~~

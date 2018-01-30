# Change the password for a Solaris ILOM.

This procedure is to change the ILO admin password.
Assuming you already have a known good password for the host LOM...


### ssh directly to the LOM IP address.

~~~~
ssh root@host-lom
~~~~


### Update user password

~~~~
set /SP/users/root password
~~~~


It is also possible to update the password from the OS using the ipmitool command.
This command is available in the package: SUNWipmi. Check if you have this installed using:

~~~~
# pkginfo SUNWipmi
system      SUNWipmi ipmitool, (usr)
~~~~


### To list user accounts available:

~~~~
# /usr/sbin/ipmitool user list 1

ID Name Callin Link Auth IPMI Msg Channel Priv Limit

1 false false true NO ACCESS

2 root false false true ADMINISTRATOR
~~~~


### To update the password:

~~~~
# ipmitool user set password 2

Password for user 2:

Password for user 2:
~~~~

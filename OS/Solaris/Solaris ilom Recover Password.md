# Recovering a lost password for a Solaris ILOM

To recover a lost ILOM password:

First I assume you have OS access and the ipmi software is installed.

~~~~
# pkginfo | grep ipmi
system      SUNWipmi                         ipmitool, (usr)
system      SUNWipmir                        ipmitool, (root)
~~~~

And that the IPMI event daemon is running:

~~~~
# svcs -a | grep ipmi
online         10:44:32 svc:/network/ipmievd:default
~~~~

Now we check the user list:

~~~~
# cd /usr/sfw/bin
# ./ipmitool user list 1
    ID  Name             Callin  Link Auth  IPMI Msg   Channel Priv Limit
    1                    false   false      true       NO ACCESS
    *2   root*           false   false      true       ADMINISTRATOR
    12  radproxy         true    true       true       OPERATOR
    13  ilomint          true    true       true       ADMINISTRATOR
    14  ldapproxy        true    true       true       OPERATOR
    15  pamproxy-admin   true    true       true       ADMINISTRATOR
    16  pamproxy-oper    true    true       true       OPERATOR
~~~~

You can now see the user "root", which is assigned ID number 2.

~~~~
# ./ipmitool user set password 2
        Password for user 2:
        Password for user 2:
~~~~

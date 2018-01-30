### Install & Configure snmpd on CentOS based hosts

#### Package Installation

Install snmpd and snmpd utils packages.

~~~~

# yum -y install net-snmp  net-snmp-utils

~~~~


#### Configuration

Update snmpd default config files

~~~~

# cat /etc/snmp/snmpd.conf
syslocation  My Location
sysservices  0
syscontact   root@localhost
rocommunity  SecretPassword
agentuser    nobody
agentgroup   nobody
master       off

~~~~

And enable / start snmpd

~~~~

# systemctl list-unit-files| grep snmp
snmpd.service                                 disabled
snmptrapd.service                             disabled

# systemctl enable snmpd.service
Created symlink from /etc/systemd/system/multi-user.target.wants/snmpd.service to /usr/lib/systemd/system/snmpd.service.

# systemctl start snmpd.service

# systemctl status snmpd.service
● snmpd.service - Simple Network Management Protocol (SNMP) Daemon.
   Loaded: loaded (/usr/lib/systemd/system/snmpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-07-03 17:35:55 IST; 8s ago
 Main PID: 31215 (snmpd)
   CGroup: /system.slice/snmpd.service
           └─31215 /usr/sbin/snmpd -LS0-6d -f

Jul 03 17:35:55 server01 systemd[1]: Starting Simple Network Management Protocol (SNMP) Daemon....
Jul 03 17:35:55 server01 snmpd[31215]: NET-SNMP version 5.7.2
Jul 03 17:35:55 server01 systemd[1]: Started Simple Network Management Protocol (SNMP) Daemon..

~~~~



#### Config Examples

##### Running Shell Scripts

Use the 'exec' option in the snmpd.conf to specify a script to run e.g.

~~~~

exec shelltest /bin/sh /tmp/shtest

exec .1.3.6.1.4.1.2021.50 shelltest2 /bin/sh /tmp/shtest2
exec .1.3.6.1.4.1.2021.51 ps /bin/ps
exec .1.3.6.1.4.1.2021.52 top /usr/local/bin/top
exec .1.3.6.1.4.1.2021.53 mailq /usr/bin/mailq

~~~~

Using snmpwalk to run the script:

~~~~

# snmpwalk -v 1 localhost -c SecretPassword .1.3.6.1.4.1.2021.8

# snmpwalk -v 1 localhost -c SecretPassword .1.3.6.1.4.1.2021.50

~~~~

##### Checking Running Processes

Use the 'proc' option in snmpd.conf to check processes running on a host e.g.

~~~~

#  Make sure mountd is running
proc mountd

#  Make sure there are no more than 4 ntalkds running, but 0 is ok too.
proc ntalkd 4

#  Make sure at least one sendmail, but less than or equal to 10 are running.
proc sendmail 10 1

~~~~

Using snmpwalk to check the process status:

~~~~

# snmpwalk -v 1 localhost -c SecretPassword .1.3.6.1.4.1.2021.2

~~~~


##### Checking Disk Status

Use the 'disk' option to check the status of a mount point in the system.

~~~~

# The agent can check the amount of available disk space, and make sure it is above a set limit.
# disk PATH [MIN=100000]
#
# PATH:  mount path to the disk in question.
# MIN:   Disks with space below this value will have the Mib's errorFlag set. Default value = 100000.

# Check the / partition and make sure it contains at least 10 megs.
disk / 10000

~~~~

Using snmpwalk to check the disk status

~~~~

# snmpwalk -v 1 localhost -c SecretPassword .1.3.6.1.4.1.2021.9

~~~~


##### Monitoring Log Files

Use the 'file' and 'logmatch' options to monitor log files.
'file' monitors the actual size in KB of a file. It sets 'fileErrorFlag' to 1 and sets a description in 'fileErrorMsg' if exceeded.
'logmatch' checks for a specific pattern in a file.

Format:

file FILE [MAXSIZE]
logmatch NAME FILE CYCLETIME REGEX

~~~~

file /usr/local/apache/logs/access.log 1000
logmatch apache-GETs /usr/local/apache/logs/access.log-%Y-%m-%d 60 GET.*HTTP.*

~~~~

The 'logmatch' option here checks the apache access.log.<date> every 60 seconds for 'GET.*HTTP.*' regex


##### snmpwalk / snmpget examples

~~~~

# snmpget -v1 -c SecretPassword localhost SNMPv2-MIB::sysDescr.0

# snmpget -v1 -c SecretPassword localhost HOST-RESOURCES-MIB::hrMemorySize.0 (.1.3.6.1.2.1.25.2.2.0)

# snmpwalk -v 1 -c SecretPassword localhost .1.3.6.1.2.1.1.1.0 <- Get uname data

# snmpwalk -v 1 -c SecretPassword localhost .1.3.6.1.2.1.2.2.1.2 <-- Get installed NICs

~~~~




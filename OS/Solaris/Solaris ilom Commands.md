# Solaris ILOM Commands

Some details on the commands available for configuring a Sun server ILOM.

The ILOM is equivalent to the Dell iDRAC or other remote consoles.

With these systems now being so old there may be issues with directly accessing the console session via the Java Web GUI. e.g. encryption options no longer supported, Java security settings disabling it by default.


Here is the system list that provide ilo access:

### ILOM Hosts

|Family|Model|
|------|-----|
|Sun Fire|X4100 X4170 X4200 X4270 X4470 X4500 X4600 X4800|
|Sun Blade|6000 8000 X4200 X6220 X6240 X6270 X6275 X6440 T6320 T6340|
|SPARC|T5120 T5140 T5220 T5240 T5440|


### Console Access

|Command|Description|
|----------------|--------------------|
|start /SP/console|start the SP-console|
|show /SP/sessions|see the currently active sessions|
|stop /SP/console|to stop any user session|


### Reboot a System

|Command|Description|
|----------------|--------------------|
|start /SYS|start system|
|stop [-force] /SYS|stop system|
|show /SYS|shows the power status|
|reset /SYS|reset host|
|reset /SP|reset ILOM SP|
|set /HOST send_break_action=break|send break signal to the OS|
|reset /CMM|reset CMM on a blade Chassis|


### Locator commands

|Command|Description|
|----------------|--------------------|
|set /SYS LOCATE=on|set the locator on|
|set /SYS LOCATE=off|set the locator off|


### Networking Commands

|Command|Description|
|----------------|--------------------|
|show /SP/network|show network config|
|set pendingipdiscovery=static|set network parameters|
|set pendingipaddress=10.10.10.10|set network parameters|
|set pendingipnetmask=255.255.255.0|set network parameters|
|set pendingipgateway=10.10.10.1|set network parameters|
|set commitpending=true|commit network parameters|
|show /SP/network macaddress|show MAC|
|show /CMM/network|If on a Blade chassis, to check the CMM IP|


### User administration

|Command|Description|
|----------------|--------------------|
|show /SP/users|Display all the ILOM users|
|show /SP/user/admin|Display configuration settings of a specific user|
|create /SP/users/user_name password=PWD role=[administrator/operator]|create new user|
|delete /SP/users/username|Delete a user|
|set /SP/users/admin01 role=administrator|set the role of a user|
|set /SP/users/admin01|set or change password of user|


### Monitoring and logs

|Command|Description|
|----------------|--------------------|
|show /SP/logs/event/list|ILOM event log|
|show -level all -output table /SP/faultmgmt|List all hardware faults|
|show -level all -output table /SYS type==Temperature value|List all temperature sensor readings|


### Hardware info

|Command|Description|
|----------------|--------------------|
|show -level all -output table /SYS type==DIMM|show DIMMS|
|show -level all -output table /SYS type=='Host Processor'|show CPUs|
|show -l all /SYS type=='Hard Disk'|show disks|

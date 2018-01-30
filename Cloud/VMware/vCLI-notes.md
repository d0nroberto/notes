### vCLI Notes

#### Checking what VMs are running on a host

~~~~
>esxcli ... vm process list
 
vm01
   World ID: 35149
   Process ID: 0
   VMX Cartel ID: 35148
   UUID: 56 4d e8 f4 21 aa ed 4b-b6 d0 1b af 43 24 ac b0
   Display Name: vm01
   Config File: /vmfs/volumes/574c0c05-a86acb5e-4588-001d0921409d/vm01/vm01.vmx
 
VMware vCenter Server Appliance
   World ID: 33590679
   Process ID: 0
   VMX Cartel ID: 33590673
   UUID: 56 4d 24 f8 03 86 58 76-37 d6 71 d9 5c 09 63 9b
   Display Name: VMware vCenter Server Appliance
   Config File: /vmfs/volumes/574c0c05-a86acb5e-4588-001d0921409d/VMware vCenter
 Server Appliance/VMware vCenter Server Appliance.vmx
 
vm02
   World ID: 301210
   Process ID: 0
   VMX Cartel ID: 301207
   UUID: 56 4d f6 53 34 78 b9 73-5a 1b 5d e8 2f 78 8c c4
   Display Name: vm02
   Config File: /vmfs/volumes/574c0c05-a86acb5e-4588-001d0921409d/vm02/vm02.vmx
~~~~


#### Kill an unresponsive VM

~~~~
>esxcli ... vm process kill -w <Workd ID> -t <soft/hard/force>
 
soft = TERM
hard = kill -9
force = force
~~~~


#### Putting a host in maintenance mode & rebooting

~~~~
>esxcli ... system maintenanceMode get
Disabled
>esxcli ... system maintenanceMode set -e
>esxcli ... system shutdown reboot -r "Clearing up stuck VM issues"
 
>esxcli ... system shutdown poweroff -r "RAM Upgrade"
~~~~


#### Getting ESXi host hardware info

~~~~
>esxcli ... hardware platform get
 
Platform Information
   UUID: 0x44 0x45 0x4c 0x4c 0x46 0x0 0x10 0x59 0x80 0x4e 0xc7 0xc0 0x4f 0x46 0x33 0x4a
   Product Name: XXXX
   Vendor Name: XXXX
   Serial Number: XXXX
   IPMI Supported: true
 
>esxcli ... hardware memory get
Enter password:
   Physical Memory: 34354360320 Bytes
   Reliable Memory: 0 Bytes
   NUMA Node Count: 1
 
 
>storage filesystem list
 
Mount Point                                        Volume Name  UUID                              Mounted  Type            Size          Free
-------------------------------------------------  -----------  -----------------------------------  -------  ------  ------------  ------------
/vmfs/volumes/574c0c05-a86acb5e-4588-001d0921409d  datastore1   574c0c05-a86acb5e-4588-001d0921409d     true  VMFS-5  576599359488  145677615104
/vmfs/volumes/574c0c0c-09b5399e-ec37-001d0921409d               574c0c0c-09b5399e-ec37-001d0921409d     true  vfat      4293591040    4251451392
/vmfs/volumes/8530377c-2c896301-569f-2df6a06190f1               8530377c-2c896301-569f-2df6a06190f1     true  vfat       261853184      97193984
/vmfs/volumes/0f505d7b-8d46f090-0659-8352ba5b4dd1               0f505d7b-8d46f090-0659-8352ba5b4dd1     true  vfat       261853184     261844992
/vmfs/volumes/574c0bf3-6a116c24-a346-001d0921409d               574c0bf3-6a116c24-a346-001d0921409d     true  vfat       299712512      99147776
 
 
>storage nfs list
 
>storage san fc list
~~~~

#### Getting / Setting various config options

~~~~
vicfg-advcfg
vicfg-authconfig
vicfg-cfgbackup
vicfg-dns
vicfg-dumppart
vicfg-hostops
vicfg-ipsec
vicfg-iscsi
vicfg-module
vicfg-mpath
vicfg-mpath35
vicfg-nas
vicfg-nics
vicfg-ntp
vicfg-rescan
vicfg-route
vicfg-scsidevs
vicfg-snmp
vicfg-syslog
vicfg-user
vicfg-vmknic
vicfg-volume
vicfg-vswitch
 
 
vihostupdate
~~~~

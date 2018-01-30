### Unattended installation of ESXi

There are 3 ways to carry out an installation of ESXi on a host:

* Interactive Installation
* Unattended Installation
* Automatic Deployment

Here I describe the unattended install option


#### Overview

For an unattended installation you will boot the host using installation media.
The media can be on USB, CDROM/ISO etc.
The procedure is similar to using a kickstart file to install Linux.


#### Instructionns

Boot your host off the installation media.
During the ESXi install boot process the boot screen suggests hitting *Shift+O* to edit boot options.

You can now specify the path to a kickstart file which can be located on an NFS mount, webserver etc.


#### Boot Options

Example kickstart commands:

~~~~
ks=usb

ks=cdrom://ks.cfg

ks=file://etc/vmware/weasel/ks.cfg (the default ks.cfg file on host RAM disk)

ks=http://build_server/host/ks.cfg ip=1.2.3.4 gateway=1.2.3.1 netmask=255.255.255.0 nameserver=1.2.3.2

~~~~


#### Kickstart File Config

The default ks.cfg contains the following options:

~~~~

#
# Sample scripted installation file
#

# Accept the VMware End User License Agreement
vmaccepteula

# Set the root password for the DCUI and Tech Support Mode
rootpw mypassword

# Install on the first local disk available on machine
install --firstdisk --overwritevmfs

# Set the network to DHCP on the first network adapter
network --bootproto=dhcp --device=vmnic0

# A sample post-install script
%post --interpreter=python --ignorefailure=true
import time
stampFile = open('/finished.stamp', mode='w')
stampFile.write( time.asctime() )

~~~~

Options:

install, upgrade, or installorupgrade. 

accepteula or vmaccepteula (required)
Accepts the ESXi license agreement.

clearpart 
(optional) - Clears any existing partitions on the disk. Requires install command to be specified.
 --drives= - Remove partitions on the specified drives.
 --alldrives - Ignores the --drives= requirement and allows clearing of partitions on every drive.
 --ignoredrives= - Removes partitions on all drives except those specified. 
 --overwritevmfs - Permits overwriting of VMFS partitions on the specified drives. 

--firstdisk=disk-type1 [disk-type2,...] 

Partitions the first eligible disk found. By default, the eligible disks are set to the following order:
1 - Locally attached storage (local)
2 - Network storage (remote)
3 - USB disks (usb)

You can change the order of the disks by using a comma separated list appended to the argument. 
If you provide a filter list, the default settings are overridden. 
You can combine filters to specify a particular disk, including esx for the first disk with ESX installed on it, model and vendor information, or the name of the vmkernel device driver. 
For example, to prefer a disk with the model name ST3120814A and any disk that uses the mptsas driver rather than a normal local disk, 
the argument is --firstdisk=ST3120814A,mptsas,local.

dryrun (optional)
Parses and checks the installation script. Does not perform the installation.

install
Specifies that this is a fresh installation.

--disk= or --drive=
Specifies the disk to partition. In the command --disk=diskname, the diskname can be in any of the forms shown in the following examples:

Path: --disk=/vmfs/devices/disks/mpx.vmhba1:C0:T0:L0
MPX name: --disk=mpx.vmhba1:C0:T0:L0
VML name: --disk=vml.000000034211234
vmkLUN UID: --disk=vmkLUN_UID

For accepted disk name formats, see Disk Device Names.

--firstdisk=

disk-type1,

[disk-type2,...]
	

Partitions the first eligible disk found. By default, the eligible disks are set to the following order:
1 - Locally attached storage (local)
2 - Network storage (remote)
3 - USB disks (usb)

You can change the order of the disks by using a comma separated list appended to the argument. 
If you provide a filter list, the default settings are overridden. 
You can combine filters to specify a particular disk, including esx for the first disk with ESX installed on it, 
model and vendor information, or the name of the vmkernel device driver. 
For example, to prefer a disk with the model name ST3120814A and any disk that uses the mptsas driver rather than a normal local disk, 
the argument is --firstdisk=ST3120814A,mptsas,local.

--overwritevmfs
Required to overwrite an existing VMFS datastore on the disk before installation.

--preservevmfs
Preserves an existing VMFS datastore on the disk during installation.

--novmfsondisk
Prevents a VMFS partition from being created on this disk. 
Must be used with --overwritevmfs if a VMFS partition already exists on the disk.

installorupgrade
Either the install, upgrade, or installorupgrade command is required to determine which disk to install or upgrade ESXi on.

--disk= or --drive=
	

Specifies the disk to partition. In the command --disk=diskname, the diskname can be in any of the forms shown in the following examples:
Path: --disk=/vmfs/devices/disks/mpx.vmhba1:C0:T0:L0
MPX name: --disk=mpx.vmhba1:C0:T0:L0
VML name: --disk=vml.000000034211234
vmkLUN UID: --disk=vmkLUN_UID

For accepted disk name formats, see Disk Device Names.

--firstdisk=disk-type1,[disk-type2,...]
	
Partitions the first eligible disk found. By default, the eligible disks are set to the following order:
1 - Locally attached storage (local)
2 - Network storage (remote)
3 - USB disks (usb)

You can change the order of the disks by using a comma separated list appended to the argument. 
If you provide a filter list, the default settings are overridden. You can combine filters to specify a particular disk, 
including esx for the first disk with ESX installed on it, 
model and vendor information, or the name of the vmkernel device driver. 
For example, to prefer a disk with the model name ST3120814A and any disk that uses the mptsas driver 
rather than a normal local disk, the argument is --firstdisk=ST3120814A,mptsas,local.

--overwritevmfs
Install ESXi if a VMFS partition exists on the disk, but no ESX or ESXi installation exists. Unless this option is present, the installer will fail if a VMFS partition exists on the disk, but no ESX or ESXi installation exists.

--forcemigrate
If the host contains customizations, such as third-party VIBS or drivers, that are not included in the installer .ISO, the installer exits with an error describing the problem. The forcemigrate option overrides the error and forces the upgrade.
Caution
Using the forcemigrate option might cause the upgraded host to not boot properly, to exhibit system instability, or to lose functionality.

keyboard (optional)
Sets the keyboard type for the system.

keyboardType
Specifies the keyboard map for the selected keyboard type. keyboardType must be one of the following types.
United Kingdom

network (optional)
Specify a network address for the system.
--bootproto=[dhcp|static]
	Specify whether to obtain the network settings from DHCP or set them manually.
--device=
	Specifies either the MAC address of the network card or the device name, in the form vmnicNN, as in vmnic0. This options refers to the uplink device for the virtual switch.
--ip=
	Sets an IP address for the machine to be installed, in the form xxx.xxx.xxx.xxx. Required with the --bootproto=static option and ignored otherwise.
--gateway=
	Designates the default gateway as an IP address, in the form xxx.xxx.xxx.xxx. Used with the --bootproto=static option.
--nameserver=
	Designates the primary name server as an IP address. Used with the --bootproto=static option. Omit this option if you do not intend to use DNS.
  The --nameserver option can accept two IP addresses. For example: --nameserver="10.126.87.104[,10.126.87.120]"
--netmask=
	Specifies the subnet mask for the installed system, in the form 255.xxx.xxx.xxx. Used with the --bootproto=static option.
--hostname=
	Specifies the host name for the installed system.
--vlanid= vlanid
	Specifies which VLAN the system is on. Used with either the --bootproto=dhcp or --bootproto=static option. Set to an integer from 1 to 4096.
--addvmportgroup=(0|1)
	Specifies whether to add the VM Network port group, which is used by virtual machines. The default value is 1.

paranoid (optional)
Causes warning messages to interrupt the installation. If you omit this command, warning messages are logged.

part or partition (optional)
Creates an additional VMFS datastore on the system. Only one datastore per disk can be created. Cannot be used on the same disk as the install command. Only one partition can be specified per disk and it can only be a VMFS partition

datastore name
Specifies where the partition is to be mounted
--ondisk= or --ondrive=
	Specifies the disk or drive where the partition is created.
--firstdisk=disk-type1,[disk-type2,...]
	Partitions the first eligible disk found. By default, the eligible disks are set to the following order:
1 - Locally attached storage (local)
2 - Network storage (remote)
3 - USB disks (usb)

You can change the order of the disks by using a comma separated list appended to the argument. 
If you provide a filter list, the default settings are overridden. 
You can combine filters to specify a particular disk, including esx for the first disk with ESX installed on it, 
model and vendor information, or the name of the vmkernel device driver. 
For example, to prefer a disk with the model name ST3120814A and any disk that uses the mptsas driver rather 
than a normal local disk, the argument is --firstdisk=ST3120814A,mptsas,local.

reboot (optional)
Reboots the machine after the scripted installation is complete.
<--noeject>
	The CD is not ejected after the installation.

rootpw (required)
Sets the root password for the system.
--iscrypted
	Specifies that the password is encrypted.

password
	Specifies the password value.
  
upgrade
Either the install, upgrade, or installorupgrade command is required to determine which disk to install or upgrade ESXi on.

--disk= or --drive=
	

Specifies the disk to partition. In the command --disk=diskname, the diskname can be in any of the forms shown in the following examples:
Path: --disk=/vmfs/devices/disks/mpx.vmhba1:C0:T0:L0
MPX name: --disk=mpx.vmhba1:C0:T0:L0
VML name: --disk=vml.000000034211234
vmkLUN UID:--disk=vmkLUN_UID

For accepted disk name formats, see Disk Device Names.

--firstdisk=disk-type1,[disk-type2,...]
	
Partitions the first eligible disk found. By default, the eligible disks are set to the following order:
1 - Locally attached storage (local)
2 - Network storage (remote)
3 - USB disks (usb)

You can change the order of the disks by using a comma separated list appended to the argument. 
If you provide a filter list, the default settings are overridden. 
You can combine filters to specify a particular disk, including esx for the first disk with ESX installed on it, model and vendor information, or the name of the vmkernel device driver. 
For example, to prefer a disk with the model name ST3120814A and any disk that uses the mptsas driver rather than a normal local disk, the argument is --firstdisk=ST3120814A,mptsas,local.

--deletecosvmdk
	

If the system is being upgraded from ESX, remove the directory that contains the old Service Console VMDK file, cos.vmdk, to reclaim unused space in the VMFS datastore.

--forcemigrate
	

If the host contains customizations, such as third-party VIBS or drivers, that are not included in the installer .ISO, the installer exits with an error describing the problem. The forcemigrate option overrides the error and forces the upgrade.
Caution

Using the forcemigrate option might cause the upgraded host to not boot properly, to exhibit system instability, or to lose functionality.
%include or include (optional)

Specifies another installation script to parse. This command is treated similarly to a multiline command, but takes only one argument.

filename
	

For example: %include part.cfg
%pre (optional)

Specifies a script to run before the kickstart configuration is evaluated. For example, you can use it to generate files for the kickstart file to include.

--interpreter

=[python|busybox]
	

Specifies an interpreter to use. The default is busybox.
%post (optional)

Runs the specified script after package installation is complete. If you specify multiple %post sections, they run in the order that they appear in the installation script.

--interpreter

=[python|busybox]
	

Specifies an interpreter to use. The default is busybox.

--timeout=secs
	

Specifies a timeout for running the script. If the script is not finished when the timeout expires, the script is forcefully terminated.

--ignorefailure

=[true|false]
	

If true, the installation is considered a success even if the %post script terminated with an error.
%firstboot

Creates an init script that runs only during the first boot. The script has no effect on subsequent boots. If multiple %firstboot sections are specified, they run in the order that they appear in the kickstart file.
Note

You cannot check the semantics of %firstboot scripts until the system is booting for the first time. A %firstboot script might contain potentially catastrophic errors that are not exposed until after the installation is complete.

--interpreter

=[python|busybox]
	
Specifies an interpreter to use. The default is busybox.

Note
You cannot check the semantics of the %firstboot script until the system boots for the first time. If the script contains errors, they are not exposed until after the installation is complete.





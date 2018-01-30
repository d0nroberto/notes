# Performing a Firmware Upgrade on a Solaris X4100 etc. 


Some things you should be aware of before you carry out a firmware upgrade. 
There are plenty of caveats so you really should visit the Oracle site and RTFM before proceeding.
e.g. the firmware upgrades need to be carried out in a specific order. 
You cannot jump from firmware version 1.0 to 1.5.0 without upgrading first to 1.1. 
You need to check the firmware README for more details.

Also is your server an *X4100* or an *X4100 M2*?.....


There are a number of ways to carry out this procedure. Probably the easiest way is through the web GUI.


If the server OS is running, perform a *clean shutdown* before proceeding.

~~~~
# shutdown -y -i5 -g20
~~~~


### Web GUI Firmware Upgrade Procedure


This is an easy one. Logon to ILOM web GUI.

In the System Maintenance tab select sub-tab *Firmware Upgrade*

Browse to the file e.g. ilom.X4100-2.0.2.5-r37165.ima and click *Upload*

Progress should be displayed followed by *Upgrade Complete* message.


### Loading the image from the SP

From the ilo, use the load command to transfer an image file from a source.
Only the TFTP protocol is supported, so the URL must begin with tftp://.
Have you got a TFTP server on the same VLAN as the SP?
Try this in Solaris:

Edit */etc/inet/inetd.conf* to uncomment tftpd

~~~~
# svcadm enable /network/tftp/udp6
~~~~


Place your image in the */tftpboot* folder & you should be ready to go.

If credentials are required and not specified, the command prompts you for a password.

~~~~
Syntax

load -source URL

Options

[-x|examine] [-h|help] [-source]

Example

load -source tftp://archive/newmainimage
~~~~

~~~~
Are you sure you want to load the specified file (y/n)? y

File upload is complete.

Firmware image verification is complete.

Do you want to preserve the configuration (y/n)? n

Updating firmware in flash RAM:

.

Firmware update is complete.

ILOM will now be restarted with the new firmware.
~~~~


An upgrade takes about *20 minutes* to complete.

The ILOM will enter a special mode to load new firmware.

No other tasks can be performed in ILOM until the firmware upgrade is complete and ILOM is reset.

# Updating the disk boot-archive on a Solaris x86 host.

### Without a reboot:

~~~~
# svcs /system/boot-archive
STATE          STIME    FMRI
online         Jan_18   svc:/system/boot-archive:default
~~~~

If the service is degraded, clear the service and update the boot archive:

~~~~
# svcadm clear system/boot-archive

# bootadm update-archive
~~~~



### Cannot boot from local disk?

Boot to failsafe mode to update a corrupt boot-archive

Reboot the box
When the boot sequence begins, the GRUB menu is displayed.

~~~~
+-------------------------------------------------------------------------+
| Solaris 10.1... X86                                                     |
| Solaris failsafe                                                        |
|                                                                         |
|                                                                         |
+-------------------------------------------------------------------------+
~~~~

Use the arrow keys to navigate the GRUB menu, then select the Solaris failsafe entry. 
Type *b* or press Enter to boot the failsafe archive.
If any boot archives are out of date, a message that is similar to the following is displayed:

~~~~
Searching for installed OS instances...
An out of sync boot archive was detected on /dev/dsk/c0t0d0s0.
The boot archive is a cache of files used during boot and
should be kept in sync to ensure proper system operation.
Do you wish to automatically update this boot archive? [y,n,?]
~~~~

Type y, then press Enter to update the out-of-date boot archive.
The system displays the following message:

~~~~
Updating boot archive on /dev/dsk/c0t0d0s0.
The boot archive on /dev/dsk/c0t0d0s0 was updated successfully.
~~~~

If no out-of-date boot archives are found, a message that is similar to the following is displayed:

~~~~
Searching for installed OS instances...
Solaris 10.1... X86 was found on /dev/dsk/c0t0d0s0.
Do you wish to have it mounted read-write on /a? [y,n,?]
~~~~

This message is also displayed after any out-of-date boot archives are updated successfully.

Mount the device that contains the corrupt boot archive on /a by typing the corresponding number
of the device, then press Enter.

To forcibly update the corrupt boot archive, type:

~~~~
# bootadm update-archive -f -R /a
~~~~

Unmount the device.

~~~~
# umount /a
~~~~

Reboot the system.

~~~~
# init 6
~~~~

This should work for hosts either using raw devices or ZFS zpools.

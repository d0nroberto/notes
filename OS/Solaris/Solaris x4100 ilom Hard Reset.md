# Factory reset an X4100 ILOM

If you can poweroff the host & take the lid off easily (i.e. the host isn't remote to you) probably the easiest way to 
reset the ILOM pasword is through the jumper pins on the motherboard.
The server model determines which jumpers you use, X4100 or X4100 M2.

After the reset, the password defaults back to *changeme*.

1. Power off the host

2. Insert a jumper (or shunt) onto the correct on-board pin.

3. Power on the host

4. Power off the host again

5. Remove the jumper

6. Power on again

In Sun Fire X4100/X4200 servers this jumper is *P4*.

In Sun Fire X4100 M2/X4200 M2 servers this is jumper *J12*. J9 - CMOS Clear. J13 - Force Recovery.

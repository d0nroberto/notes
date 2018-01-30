# Configure the network IP address for a Solaris ILOM

Assuming you are not working with a blade chassis with a CMM module.


### Establish a serial connection to the ILOM.

Using terminal emulator software / TIP server to access 'SER MGT' port


### Log in to the ILOM.

Do you have a password? The default is 'changeme' IIRC.


### Type the following command to set the working directory.

~~~~
cd /SP/network
~~~~


### Type the following commands to specify a static Ethernet configuration.

~~~~
set pendingipaddress=[your_ilom_IP]

set pendingipnetmask=[your_ilom_NM]

set pendingipgateway=[your_ilom_GW]

set commitpending=true
~~~~


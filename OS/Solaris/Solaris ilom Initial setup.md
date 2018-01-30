# Solaris ILOM Initial setup

Assuming that you have the host racked / cabled / IPs ready to go etc.
Another assumption I make is that you are working with pizza box systems and not using a blade chassis with CMM.

### Connect through a Serial Console:

These systems typically have an RJ45 *'SERIAL MGT'* port at the back.
You will also need a *'DIGI CM CONSOLE ADAPTER #76000671'* and a LAN cable.
The digi connector will have the correct pin out to attach to your serial port.
Using terminal emulator software to manage the connection use the following settings:

~~~~
Bits Per Second - 9600
Data Bits - 8
Parity- None
Stop Bits - 1
Flow Control - None
~~~~

Now login to the ILOM with the default credentials *root / changeme*.


### Configure networking:

At the ILO prompt, navigate to /SP/network

~~~~
cd /SP/network
~~~~

Configure IP & gateway

~~~~
-> set pendingipaddress=192.168.179.55
-> set pendingipnetmask=255.255.255.0
-> set pendingipgateway=192.168.179.1
-> set commitpending=true
~~~~

Log out of the ILOM port

Now connect the Service Processor Ethernet port to the network.
You should be able to access it by using ssh or through your web browser.

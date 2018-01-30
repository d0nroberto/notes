### Ubuntu Set a Static IP Address for a Host

ifconfig -a should show a list of all available interfaces.

~~~~
# ifconfig -a
...
ens160: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
...
~~~~


Edit the file /etc/netword/interfaces setting the various network options.

To add a static IP add the following lines:

~~~~
iface ens160 inet static
	address [ip address]
	gateway [gateway ip]
	netmask [netmask]
	dns-nameservers [nameserver1 nameserver2]
	dns search [domain1 domain2]
~~~~

And now up the interface:

~~~~
# ifup ens160
~~~~

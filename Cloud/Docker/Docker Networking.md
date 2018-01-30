### Docker Networking

# The docker0 Virtual Bridge



When the docker daemon is running it creates a virtual interface "docker0"

<code># ifconfig -a
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 0.0.0.0
        inet6 fe80::42:b8ff:fe4d:14a0  prefixlen 64  scopeid 0x20<link>
        ether 02:42:b8:4d:14:a0  txqueuelen 0  (Ethernet)
        RX packets 45998  bytes 2584664 (2.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 76108  bytes 173165195 (165.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens32: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
…

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
…</code>

docker0 acts as a virtual switch which can allow communications between containers.

Use the brctl command to see the status of the virtual switch & the network interfaces attached to it.
	
<code># yum provides brctl
…
bridge-utils-1.5-9.el7.x86_64 : Utilities for configuring the linux ethernet bridge

# brctl show docker0
bridge name     bridge id               STP enabled     interfaces

# docker run -d webserver:0.1
712c246bf1ec361a78f2c51bdf279a23086eef0b69c8763b9089dc1a513a261b
#

# brctl show docker0
bridge name     bridge id               STP enabled     interfaces
docker0         8000.0242b84d14a0       no              vethbefbe88</code>


# Virtual Ethernet Interfaces

A container is assigned a Virtual Ethernet interface vethxxx e.g.

<code># ifconfig -a
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 0.0.0.0
…
vethbefbe88: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::2866:d2ff:fedd:5b8b  prefixlen 64  scopeid 0x20<link>
        ether 2a:66:d2:dd:5b:8b  txqueuelen 0  (Ethernet)</code>
	
Docker chooses an address range that is available in the reserved address class e.g.:

<code>172.17.0.1/16
172.17.42.1/24
172.17.43.1/24
172.17.44.1/24
192.168.42.1/24
192.168.43.1/24
192.168.44.1/24
10.0.42.1/24
10.1.42.1/24</code>

The default gateway for the container is the docker0 virtual switch.

<code># docker inspect 712c246bf1ec
…
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "b5725674b4f8521416d421e524cf498926f7114ddc6e8d37425878a905d3d487",
                    "EndpointID": "61d2adf1a4b80c1bb4032e612be56bfc6956770e2c90a72b20015f4809514cb7",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02"
…</code>

You can manually select which address range should be used by setting the Gateway IP option in the 'dockerd' command:
	
--bip=""
Use the provided CIDR notation address for the dynamically created bridge (docker0)
	
e.g.

<code>--bip=10.1.42.1/24</code>


From within the container, the NIC iss assigned the prefix 'eth' e.g.

<code># docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
0cb35f346277        webserver:0.1       "apache2ctl -D FOR..."   21 hours ago        Up 21 hours         0.0.0.0:80->80/tcp   blissful_goldwasser

# docker exec -it 0cb35f346277 /bin/bash

# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
106: eth0@if107: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe11:2/64 scope link
       valid_lft forever preferred_lft forever</code>


# DNS Settings

The resolv.conf is taken from the docker host and copied directly to the container into:

	/var/lib/docker/containers/
	
This can be edited on the fly or by passing parameters to 'docker run'

<code># docker run --dns=8.8.8.8 webserver:0.1</code>


# Exposing Ports

To expose ports in a docker container use the 'EXPOSE' instruction in Dockerfile.

<code>EXPOSE 10051</code>

Then use the -p option to map the exposed tport to a port on the docker host e.g.

<code># docker run -d -p 10051:10051 zabbix_server</code>

You can also use the -P command to map all ports automatically.
Use the 'docker port' command to see the mapping table.


# Linking Containers

Linking containers allows us to let containers communicate with each other without communicating with the outside world.
Only for Container -> Container.

There is a source container and receiver container.

The source container uses the Dockerfile EXPOSE instruction to export a port.

<code>EXPOSE 8080</code>

The receiver container is run with the --link option to docker run.

--link=[]
Add link to another container in the form of <name or id>:alias 
or just <name or id> in which case the alias will match the name
		
If the operator uses --link when starting the new client container, 
then the client container can access the exposed port via a private networking interface. 
Docker will set some environment variables in the client container to help indicate which interface and port to use.


<code># docker run -d --name=web01 --link=db01:zabbix_db webserver:0.1</code>

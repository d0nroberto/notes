### Apache Zookeeper Getting Started

Apache Zookeeper is a 'highly reliable distributed coordination' service.
Zookeeper servers provide distributed configuration information to services.

It is a java based service.
It provides a filesystem tree-like directory of configuration information to other services.
The data it maintains is stored in 'z-nodes' (zookeeper data nodes). The z-node can function as a file and a directory at the same time.
The data is also stored in-memory and written to disk.

It provides high performance, highly available, strictly ordered access.


#### Architecture

A number of Zookeeper servers replicate data across each other. The servers are known as an ensemble.
One server is the leader of the cluster. The rest are the follower servers.

A client talks to a single zookeeper server in the cluster. The server will handle all read requests from the client.
Any write requests are then forwarded to the leader server.

The leader server then communicates with all the followers to make sure they are all updated.

Its API provides a very limited set of functions:

create - create a znode

delete - delete a znode

exists - check if a znode exists

get data - read from a znode

set data - write to a znode

get children - get a list of all children znodes

sync - propagate data to cluster


#### Installation

Download from [here](http://www-eu.apache.org/dist/zookeeper/stable/)

~~~~
cd /opt
wget http://www-eu.apache.org/dist/zookeeper/stable/zookeeper-3.4.10.tar.gz
gunzip zookeeper-3.4.10.tar.gz
tar -xvf zookeeper-3.4.10.tar
mv zookeeper-3.4.10 zookeeper
cd zookeeper/conf
cp zoo_sample.conf zoo.conf
vim zoo.cfg
cd ../
bin/zkServer.sh status
bin/zkServer.sh start
bin/zkServer.sh status
~~~~

~~~~
bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/zookeeper/bin/../conf/zoo.cfg
Error contacting service. It is probably not running.

bin/zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /opt/zookeeper/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED

bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/zookeeper/bin/../conf/zoo.cfg
Mode: standalone
~~~~


#### Configuration

The default config file for zookeeper is ./conf/zoo.cfg
A default config typically looks like:

~~~~
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
~~~~

tickTime - The number of milliseconds of each 'tick'
initLimit - The number of ticks that the initial synchronization phase can take. How long a member has to connect to the leader = tickTime * initLimit (2000 * 5 = 10 seconds)
syncLimit - The number of ticks that can pass between sending a request and getting an acknowledgement. Timeout Limit = tickTime * syncLimit (2000 * 2 = 4 seconds)


#### Connecting To A Server

Use the zkCli.sh script to open a session with a server.

~~~~
# bin/zkCli.sh -server 127.0.0.1:2181
Connecting to 127.0.0.1:2181
2017-06-28 15:33:01,610 [myid:] - INFO [main:Environment@100] - Client environment:host.name=test01.example.com
2017-06-28 15:33:01,610 [myid:] - INFO [main:Environment@100] - Client environment:java.version=1.8.0_131
2017-06-28 15:33:01,614 [myid:] - INFO [main:Environment@100] - Client environment:java.vendor=Oracle Corporation
2017-06-28 15:33:01,614 [myid:] - INFO [main:Environment@100] - Client environment:java.home=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.131-3.b12.el7_3.x86_64/jre
2017-06-28 15:33:01,615 [myid:] - INFO [main:Environment@100] - Client environment:java.class.path=/opt/zookeeper/bin/../build/classes:/opt/zookeeper/bin/../build/lib/*.jar:/opt/zookeeper/bin/../lib/slf4j-log4j12-1.6.1.jar:/opt/zookeeper/bin/../lib/slf4j-api-1.6.1.jar:/opt/zookeeper/bin/../lib/netty-3.10.5.Final.jar:/opt/zookeeper/bin/../lib/log4j-1.2.16.jar:/opt/zookeeper/bin/../lib/jline-0.9.94.jar:/opt/zookeeper/bin/../zookeeper-3.4.10.jar:/opt/zookeeper/bin/../src/java/lib/*.jar:/opt/zookeeper/bin/../conf:
2017-06-28 15:33:01,615 [myid:] - INFO [main:Environment@100] - Client environment:java.library.path=/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
2017-06-28 15:33:01,615 [myid:] - INFO [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2017-06-28 15:33:01,615 [myid:] - INFO [main:Environment@100] - Client environment:java.compiler=<NA>
2017-06-28 15:33:01,615 [myid:] - INFO [main:Environment@100] - Client environment:os.name=Linux
2017-06-28 15:33:01,616 [myid:] - INFO [main:Environment@100] - Client environment:os.arch=amd64
2017-06-28 15:33:01,616 [myid:] - INFO [main:Environment@100] - Client environment:os.version=3.10.0-514.16.1.el7.x86_64
2017-06-28 15:33:01,616 [myid:] - INFO [main:Environment@100] - Client environment:user.name=root
2017-06-28 15:33:01,616 [myid:] - INFO [main:Environment@100] - Client environment:user.home=/root
2017-06-28 15:33:01,616 [myid:] - INFO [main:Environment@100] - Client environment:user.dir=/opt/zookeeper
2017-06-28 15:33:01,618 [myid:] - INFO [main:ZooKeeper@438] - Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@3eb07fd3
Welcome to ZooKeeper!
2017-06-28 15:33:01,649 [myid:] - INFO [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1032] - Opening socket connection to server 127.0.0.1/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
JLine support is enabled
2017-06-28 15:33:01,731 [myid:] - INFO [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@876] - Socket connection established to 127.0.0.1/127.0.0.1:2181, initiating session
[zk: 127.0.0.1:2181(CONNECTING) 0] 2017-06-28 15:33:01,791 [myid:] - INFO [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1299] - Session establishment complete on server 127.0.0.1/127.0.0.1:2181, sessionid = 0x15cef10ecc50001, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

[zk: 127.0.0.1:2181(CONNECTED) 0] ?
ZooKeeper -server host:port cmd args
       stat path [watch]
       set path data [version]
       ls path [watch]
       delquota [-n|-b] path
       ls2 path [watch]
       setAcl path acl
       setquota -n|-b val path
       history
       redo cmdno
       printwatches on|off
       delete path [version]
       sync path
       listquota path
       rmr path
       get path [watch]
       create [-s] [-e] path data acl
       addauth scheme auth
       quit
       getAcl path
       close
       connect host:port
[zk: 127.0.0.1:2181(CONNECTED) 1]
~~~~


#### Create / modify / delete a Znode

~~~~
# ls /

# create /zkdata zkvalue

# ls /

# ls /zkdata

# get /zkdata

# set /zkdata zkvalue_new

# get /zkdata

# delete /zkdata

# ls /
~~~~


#### Zookeeper Replication

In replication mode there need to be a number of instances of zookeeper running.
To have a quorum you require at least 3 servers.

The servers taking part in the cluster are listed in the zookeeper config file e.g. ./conf/zoo.cfg

~~~~
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
server.1=zookeeper01:2888:3888
server.2=zookeeper02:2888:3888
server.3=zookeeper03:2888:3888
~~~~


tickTime - The number of milliseconds of each 'tick'
initLimit - The number of ticks that the initial synchronization phase can take. How long a member has to connect to the leader = tickTime * initLimit (2000 * 5 = 10 seconds)
syncLimit - The number of ticks that can pass between sending a request and getting an acknowledgement. Timeout Limit = tickTime * syncLimit (2000 * 2 = 4 seconds)
server.X - A list of the servers that make up the ZooKeeper service.

Port 2888 - Used for communication between cluster members
Port 3888 - Used for leader election

If you are testing zookeeper and want to run multiple instances on the same host, modify the config to use 'localhost' and allocate each instance separate port ranges for the servers as well as separate data dirs etc..
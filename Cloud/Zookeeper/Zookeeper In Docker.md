
### Apache Zookeeper using Docker Images

There is an official supported docker image available for running zookeeper instances.

More details can be found (here)[https://hub.docker.com/_/zookeeper/].

#### Running the Image

Simple run command for the image is:

~~~~

docker run --name zookeeper_node --restart=always -d zookeeper

~~~~


To allow another container to access this container you will need to provide the --link option e.g. --link=zookeeper_node

The zookeeper container exposes the following ports: 2181 2888 and 3888

If network access is required they will need to be published e.g. -p 2182:2181 -p 2888:2888 -p 3888:3888

You may also need to allow ports if firewalld is running on a host e.g.

~~~~

firewall-cmd --add-port 2181/tcp
firewall-cmd --add-port 2181/tcp --permanent
firewall-cmd --add-port 2888/tcp
firewall-cmd --add-port 2888/tcp --permanent
firewall-cmd --add-port 3888/tcp
firewall-cmd --add-port 3888/tcp --permanent

~~~~


The performance of the zookeeper cluster may depend on the location of the 'dataDir' and 'dataLogDir' folders used by the containers.

You may need to use volumes to have them use dedicated storage e.g. -v /opt/docker/zookeeper_node/data:/data -v /opt/docker/zookeeper_node/datalog:/datalog

If clustering the zookeeper nodes you will need to provide two environment variables to the run command: ZOO_MY_ID and ZOO_SERVERS.

The ZOO_MY_ID var sets the value of the /data/myid file which needs to be unique for each cluster member.

The ZOO_SERVERS var sets the server variable in the zookeeper config to include all servers in the cluster.

~~~~

Run on node 1:
docker run --hostname=`hostname` -e "ZOO_MY_ID=1" -e "ZOO_SERVERS=server.1=test01:2888:3888 server.2=test02:2888:3888 server.3=test03:2888:3888" -p 2181:2181 -p 2888:2888 -p 3888:3888 --name zookeeper_node --restart=always -d zookeeper

Run on node 2:
docker run --hostname=`hostname` -e "ZOO_MY_ID=2" -e "ZOO_SERVERS=server.1=test01:2888:3888 server.2=test02:2888:3888 server.3=test03:2888:3888" -p 2181:2181 -p 2888:2888 -p 3888:3888 --name zookeeper_node --restart=always -d zookeeper

Run on node 3:
docker run --hostname=`hostname` -e "ZOO_MY_ID=3" -e "ZOO_SERVERS=server.1=test01:2888:3888 server.2=test02:2888:3888 server.3=test03:2888:3888" -p 2181:2181 -p 2888:2888 -p 3888:3888 --name zookeeper_node --restart=always -d zookeeper

~~~~

You can connect to the containers and run the zkCli.sh client or telnet commands as normal.
# Docker Swarm Mode

Swarm Mode allows you to deploy a number of containers across multiple hosts using the docker command.

Swarm Mode has been deployed with the docker 1.12 release.

A Docker node has an instance of the Docker Engine running and connected to the Swarm.
A node is either a manager or a worker.
Managers schedules which containers to run where.
Workers run the containers. By default, Managers are also workers.

A service is essentially a group of containers to be run on workers.
An example of a service is an HTTP Server running as a Docker Container on three nodes.

Docker includes a load-balancer to process requests across all containers in the service.
The load-balancer uses the [Linux IPVS](http://www.linuxvirtualserver.org/software/ipvs.html) kernel module.

The first node to initialise the Swarm Mode becomes the manager.
As new nodes join the cluster, they can adjust their roles between managers or workers.
You should run 3-5 managers in a production environment to ensure high availability.
Setting up a Manager

~~~~
# docker swarm --help

Usage:  docker swarm COMMAND

Manage Swarm

Options:
      --help   Print usage

Commands:
  init        Initialize a swarm
  join        Join a swarm as a node and/or manager
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

Run 'docker swarm COMMAND --help' for more information on a command.

# docker swarm init
Swarm initialized: current node (fg1bqyqzcjl9vrapbsvghmb2r) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-29kb3mm8ks8jxvnt3wko5ajoi4ryo5ssinlsaqj2qoyg79jiz8-cdv46y9tzl6mb76fc7oaklklg \
    [manager_ip]:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
~~~~

Adding a Worker:

~~~~
# docker swarm join --token SWMTKN-1-29kb3mm8ks8jxvnt3wko5ajoi4ryo5ssinlsaqj2qoyg79jiz8-cdv46y9tzl6mb76fc7oaklklg [manager_ip]:2377
This node joined a swarm as a worker.
~~~~

To query the token from the worker:

~~~~
# docker -H [manager_ip] swarm join-token -q worker
~~~~

Checking to see the worker joined the swarm from the manager:

~~~~
# docker node ls
ID                           HOSTNAME STATUS  AVAILABILITY  MANAGER STATUS
906mwo2kynl2xen18ydaggad4    node02   Ready   Active
fg1bqyqzcjl9vrapbsvghmb2r *  node01   Ready   Active        Leader
~~~~

### Create an Overlay Network

The overlay network allows containers on different docker nodes to communicate with each other.
It does this by creating a Virtual Extensible LAN (VXLAN), designed for cloud-based networking.
The service is given a Virtual IP address that is routable only inside the Docker Network.

Run the following command on the Manager node:

~~~~
# docker network create -d overlay docker_net
r067mr8c9a12unliyg3xktqoo

# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0710d20adf43        bridge              bridge              local
1d2d174295b8        docker_gwbridge     bridge              local
r067mr8c9a12        docker_net          overlay             swarm
ff7bf345f7af        host                host                local
r41kii82ckbs        ingress             overlay             swarm
6c997ded70cc        none                null                local
~~~~

All containers registered to this network can communicate with each other.

### Deploy a Service

Docker by default uses spread scheduling to decide what node should run a container. Binpack and random are also available.
The idea of a service allows you to provide a container running on many nodes.

Deploying the Docker Image nginx to a number of nodes in the swarm:

~~~~
# docker service create --name httpd --network docker_net --replicas 2 -p 80:80 nginx
fveg68rps4cwxx646sy3od33w
~~~~
As containers are started you will see them using the ps command.

~~~~
# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

# docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
fveg68rps4cw  httpd  replicated  0/2       nginx:latest

# docker ps
CONTAINER ID        IMAGE                                                                           COMMAND                  CREATED             STATUS              PORTS               NAMES
9a03170949e0        nginx@sha256:4296639ebdf92f035abf95fee1330449e65990223c899838283c9844b1aaac4c   "nginx -g 'daemon ..."   19 seconds ago      Up 17 seconds       80/tcp, 443/tcp     httpd.1.8pouieyqdbuikte42uwm16zms

# docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
fveg68rps4cw  httpd  replicated  2/2       nginx:latest
~~~~

Sending an HTTP request to any of the nodes in the cluster will send the request to one of the containers within the swarm.
The node which accepted the request might not be the node where the container is running. 
Docker load-balances requests across all available containers.

To check the Virtual IP assigned to a service:

~~~~
# docker service inspect httpd | more
...
            "VirtualIPs": [
                {
                    "NetworkID": "r41kii82ckbsq59x2aiahm3i3",
                    "Addr": "10.255.0.2/16"
                },
                {
                    "NetworkID": "r067mr8c9a12unliyg3xktqoo",
                    "Addr": "10.0.0.2/24"
                }
            ]
...

# docker service inspect httpd --format="{{.Endpoint.VirtualIPs}}"

[{r41kii82ckbsq59x2aiahm3i3 10.255.0.2/16} {r067mr8c9a12unliyg3xktqoo 10.0.0.2/24}]

# docker inspect --format="{{.NetworkSettings.Networks,eg1,IPAddress}}" [container_id]


~~~~

~~~~
# docker exec -it 9a03170949e0 /bin/bash
root@9a03170949e0:/# echo 9a03170949e0 > /usr/share/nginx/html/index.html
root@9a03170949e0:/# exit
exit

# curl http://node01
9a03170949e0

# curl http://node01
rb8g1funkr0e

# curl http://node01
9a03170949e0

# curl http://node01
rb8g1funkr0e

# curl http://node01
9a03170949e0
~~~~

### Inspect Swarm Status

The Service command lets you to check the health of the swarm and the running containers.
You can view the details and configuration of a service via:

~~~~
# docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
fveg68rps4cw  httpd  replicated  2/2       nginx:latest

# docker service ps httpd
ID            NAME     IMAGE         NODE    DESIRED STATE  CURRENT STATE           ERROR  PORTS
8pouieyqdbui  httpd.1  nginx:latest  node01  Running        Running 39 minutes ago
zj30uulawzmb  httpd.2  nginx:latest  node02  Running        Running 39 minutes ago

# docker service inspect --pretty httpd

ID:             fveg68rps4cwxx646sy3od33w
Name:           httpd
Service Mode:   Replicated
 Replicas:      2
Placement:
UpdateConfig:
 Parallelism:   1
 On failure:    pause
 Max failure ratio: 0
ContainerSpec:
 Image:         nginx:latest@sha256:4296639ebdf92f035abf95fee1330449e65990223c899838283c9844b1aaac4c
Resources:
Networks: docker_net
Endpoint Mode:  vip
Ports:
 PublishedPort 80
  Protocol = tcp
  TargetPort = 80
~~~~

On each node, you can ask what tasks it is currently running.

~~~~
# docker node list
ID                           HOSTNAME STATUS  AVAILABILITY  MANAGER STATUS
906mwo2kynl2xen18ydaggad4    node02   Ready   Active
fg1bqyqzcjl9vrapbsvghmb2r *  node01   Ready   Active        Leader

# docker node ps self
ID            NAME     IMAGE         NODE    DESIRED STATE  CURRENT STATE           ERROR  PORTS
8pouieyqdbui  httpd.1  nginx:latest  node01  Running        Running 41 minutes ago

# docker node ps node02
ID            NAME     IMAGE         NODE    DESIRED STATE  CURRENT STATE           ERROR  PORTS
8pouieyqdbui  httpd.1  nginx:latest  node02  Running        Running 41 minutes ago
~~~~

Self refers to the manager node Leader.

A Service allows us to scale how many instances of a container are running in the cluster.

~~~~
# docker service scale httpd=5
httpd scaled to 5

# docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
fveg68rps4cw  httpd  replicated  4/5       nginx:latest

# docker service ls
ID            NAME   MODE        REPLICAS  IMAGE
fveg68rps4cw  httpd  replicated  5/5       nginx:latest

# docker node ps self
ID            NAME     IMAGE         NODE    DESIRED STATE  CURRENT STATE           ERROR  PORTS
8pouieyqdbui  httpd.1  nginx:latest  node01  Running        Running 43 minutes ago
yjdmltsuyd9r  httpd.3  nginx:latest  node01  Running        Running 8 seconds ago
rb8g1funkr0e  httpd.5  nginx:latest  node01  Running        Running 9 seconds ago

# docker node ps node02
ID            NAME     IMAGE         NODE    DESIRED STATE  CURRENT STATE           ERROR  PORTS
zj30uulawzmb  httpd.2  nginx:latest  node02  Running        Running 43 minutes ago
6zevpz8dt9yg  httpd.4  nginx:latest  node02  Running        Running 19 seconds ago
~~~~

### Remotely Accessing Docker Daemon

The docker daemon by default binds to a unix socket file /var/run/docker.sock
If you need to access a docker daemon on a remote host, you must configure docker to listen on a port using the -H option.

You can modify the docker daemon startup by editing the files /etc/docker/daemon.json or /etc/default/docker


#### /etc/docker/daemon.json

~~~~
{
"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]
}
~~~~

#### /etc/default/docker

~~~~
DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
~~~~

You will now need to restart docker.

If the docker server is also running firewalld, you will need to enable access to the port

~~~~
firewall-cmd --add-port 2375/tcp
firewall-cmd --add-port 2375/tcp --permanent
~~~~

The docker daemon is now available for remote access. You can access the daemon API programmatically or through curl.

~~~~
curl http://0.0.0.0:2375/images/json
~~~~

You can find more details on interacting with the API using e.g. curl commands [here](https://docs.docker.com/engine/api/get-started/)

To remotely access the daemon from your docker client, set the DOCKER_HOST env variable to the host you want to access:

~~~~
export DOCKER_HOST=tcp://192.168.0.25:2375
~~~~


You can now issue docker commands from your client to the remote host.

~~~~
# docker version
Client:
 Version:      17.03.2-ce
 API version:  1.27
 Go version:   go1.7.5
 Git commit:   f5ec1e2
 Built:        Tue Jun 27 02:21:36 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.03.2-ce
 API version:  1.27 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   f5ec1e2
 Built:        Tue Jun 27 02:21:36 2017
 OS/Arch:      linux/amd64
 Experimental: false
~~~~


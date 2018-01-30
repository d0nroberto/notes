# Docker Registries and Repos

A registry contains a number of repositories.
The main registry provided by docker.io is hub.docker.com

There are trusted repos for various linux distros.
There are also public repos which may or may-not be trustworthy.

You can also run a private repo within your own network.


### Public Repo

The public docker repo is available at https://hub.docker.com.
To upload an image to the docker public repo, tag the image with the repo name

~~~~
# docker login

# docker tag [image_id] [repo_name/image_name:versnum]

# docker push [repo_name/image_name:versnum]
~~~~


### Private Repo

Running your own registry:

~~~~
# docker run -d -p 5000:5000 registry
Unable to find image 'registry:latest' locally
latest: Pulling from library/registry
b7f33cc0b48e: Pull complete
46730e1e05c9: Pull complete
458210699647: Pull complete
0cf045fea0fd: Pull complete
b78a03aa98b7: Pull complete
Digest: sha256:0e40793ad06ac099ba63b5a8fae7a83288e64b50fe2eafa2b59741de85fd3b97
Status: Downloaded newer image for registry:latest
dd8c89c81cf46037c0bfac17335b2e766798012fd25f2c238a9ec630b48b903e

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
registry            latest              d1e32b95d8e8        3 weeks ago         33.2 MB

# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
dd8c89c81cf4        registry            "/entrypoint.sh /e..."   39 seconds ago      Up 38 seconds       0.0.0.0:5000->5000/tcp   stoic_snyder
~~~~

To upload a local image to your own repo:

~~~~
# docker tag [image_id] [hostname:5000]/[tag]

# docker push [hostname:5000]/[tag]
~~~~
e.g.
~~~~
 # docker tag proxy-server myregistryhost:5000/proxy/proxy-server:1.0
 # docker tag proxy-server myregistryhost:5000/proxy/proxy-server:latest
 # docker push myregistryhost:5000/zabbix3.2/proxy-server
~~~~
For insecure communication with a registry:

Ubuntu - DOCKER_OPTS="--insecure-registry [hostname:5000]" in /etc/default/docker
CentOS 0 ExecStart=/usr/bin/docker -d $OPTIONS $DOCKER_STORAGE_OPTIONS --insecure-registry [hostname:5000] in /usr/lib/systemd/system/docker.service
	
~~~~
ExecStart=/usr/bin/docker -d $OPTIONS \
          $DOCKER_STORAGE_OPTIONS \
          $DOCKER_NETWORK_OPTIONS \
          $ADD_REGISTRY \
          $BLOCK_REGISTRY \
          $INSECURE_REGISTRY
LimitNOFILE=1048576
~~~~

Other alternative registry server images / front ends:

~~~~
# docker images
konradkleine/docker-registry-frontend
hyper/docker-registry-web
~~~~

### Querying A Repo

You can use the curl command to perform queries on docker registries e.g. v2.

~~~~
  # curl http://myregistryhost:5000/v2/_catalog| tr ',' '\n'
  # curl http://myregistryhost:5000/v2/proxy/proxy-server/tags/list
~~~~

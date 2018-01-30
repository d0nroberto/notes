### Dockerfile

A Dockerfile contains all the text used to describe creating a new docker Image.

All files / folders in the directory that contains the Dockerfile gets included in the image build.

Use docker-build to create the new image using the Dockerfile.

<code># docker build -t helloworld:0.1 .</code>


Dockerfile Syntax

The Dockerfile contains a list of instructions for creating the image.

Format:

<code>INSTRUCTION [value]</code>


# The FROM Instruction

Selects the base image the container is based upon e.g.

<code>FROM ubuntu:latest</code>
	

# The RUN instruction

RUN commands are run at image Build Time
	
RUN - Run commands against an image.
	
Each RUN instruction creates a new layer on top of an image
	
<code>RUN apt-get update
		
RUN apt-get install -y apache2</code>
		

# The CMD instruction

CMD commands are run at image Launch Time

CMD - Command to run on the container

CMD is equivalent to docker run [args] [CMD]
	
This will override the CMD command in the Dockerfile.
	
Only the last CMD in a Dockerfile is run. Any others are ignored.

There are two forms of the CMD instruction:

	Shell Form - Docker prepends "/bin/sh -c" to the CMD 

	Exec Form - Pass a JSON array ["command", "arg"] e.g.
		
<code>CMD ["apache2ctl", "-D",  "FOREGROUND"]</code>


# The ENTRYPOINT instruction

Command to run on the container. Cannot be overridden like CMD.

<code>ENTRYPOINT ["echo"]</code>

Anything you pass to the container at runtime is passed to the ENTRYPOINT command.

<code>ENTRYPOINT ["zabbix_server"]</code>

<code># docker run -d -p 10051:10051 zabsrv01 -c /etc/zabbix/zabbix_server.conf</code>


# The ENV Instruction

Allow you to pass variables to a running container e.g.

<code>ENV DBName=zabbix DBUser=zabbix DBPassword=zabbix DBPort=3306</code>

They can also be used within the Dockerfile e.g.

<code>	ENV command=echo arg=Hello_World
	CMD $command $arg</code>

	You should put all variable declarations in one ENV line as each ENV line creates an image layer.

# The VOLUME Instruction

<code>VOLUME [/mount_point]</code>

The VOLUME instruction allows you to store the contents of the folder on the docker host's filesystem.
When the container is deleted with 'docker rm' the volume will remain.
To later delete the volume you must use 'docker rm -v [container]'


# The COPY Instruction

<code>COPY [src] [dest]</code>
	
The COPY instruction allows you to copy a file from the folder containing the Dockerfile to a path in the image.
	
e.g. to copy all the files in the filder ./block/ to /etc/dnsmasq.d in the container:
	
<code>COPY block /etc/dnsmasq.d/</code>

e.g. to copy a single file to a folder in the container:

<code>COPY dnsmasq.conf /etc/</code>


#The EXPOSE Instruction

This allows you to expose a port in the container to the host e.g.

	<code>EXPOSE 10051</code>

You can then allow port forwarding from the 'docker run' command e.g.

<code># docker run -d -p 10051:10051 zabbix_server

# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
712c246bf1ec        zabbix_server:0.1       "zabbix_serv..."   41 minutes ago      Up 41 minutes       80/tcp              inspiring_goldberg</code>

The command docker port should also provide info.

<code># docker run -d -p 80:80 webserver:0.1
0cb35f346277a426085a1f9705b8f715fe49733384d0f455f04bb08207f94c12

# docker port 0cb35f346277
80/tcp -> 0.0.0.0:80</code>


Dockerfile examples:

<code># cat Dockerfile
# Ubunto base OS
# Writes 'Hello World'


FROM ubuntu:15.04
MAINTAINER anonymous@example.com
RUN apt-get update
CMD ["echo","Hello World"]</code>


Docker build completes the build.
-t = tag for build image
. = Directory containing the Dockerfile

<code># docker build -t helloworld:0.1 .
Sending build context to Docker daemon 2.048 kB
Step 1/4 : FROM ubuntu:15.04
 ---> d1b55fd07600
Step 2/4 : MAINTAINER anonymous@example.com
 ---> Using cache
 ---> 0eb4506ba7a2
Step 3/4 : RUN apt-get update
 ---> Using cache
 ---> 30067d4ec3b6
Step 4/4 : CMD echo Hello World
 ---> Running in c2d9c562ead5
 ---> 626fa235c3a1
Removing intermediate container c2d9c562ead5
Successfully built 626fa235c3a1

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
helloworld          0.1                 626fa235c3a1        50 seconds ago      153 MB
ubuntu              15.04               d1b55fd07600        12 months ago       131 MB

# docker run helloworld:0.1
Hello World
#</code>


You can find examples of Dockerfiles in hub.docker.com / GitHub


Every command in a Dockerfile creates a layer in an image.


<code># cat Dockerfile

# demo Dockerfile
# Ubunto base OS
# Writes 'Hello World'

FROM ubuntu:latest
MAINTAINER anonymous@example.com
RUN apt-get update
RUN apt-get install -y vim
CMD ["echo","Hello World"]</code>



<code># docker build -t="build_02" .
Sending build context to Docker daemon 2.048 kB
Step 1/5 : FROM ubuntu:latest
 ---> f49eec89601e
Step 2/5 : MAINTAINER anonymous@example.com
 ---> Using cache
 ---> ac268036c620
Step 3/5 : RUN apt-get update
 ---> Using cache
 ---> 99b4003997c6
Step 4/5 : RUN apt-get install -y vim
 ---> Using cache
 ---> 8c40697ffb97
Step 5/5 : CMD echo Hello World
 ---> Using cache
 ---> fd48e725257a
Successfully built fd48e725257a

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
build_01            latest              fd48e725257a        About a minute ago   227 MB
build_02            latest              fd48e725257a        About a minute ago   227 MB
ubuntu              latest              f49eec89601e        3 weeks ago          129 MB

# docker history build_02
IMAGE               CREATED              CREATED BY                                      SIZE                COMMENT
fd48e725257a        About a minute ago   /bin/sh -c #(nop)  CMD ["echo" "Hello World"]   0 B
8c40697ffb97        About a minute ago   /bin/sh -c apt-get install -y vim               57.8 MB
99b4003997c6        About a minute ago   /bin/sh -c apt-get update                       39.8 MB
ac268036c620        2 minutes ago        /bin/sh -c #(nop)  MAINTAINER anonymous@ex...   0 B
f49eec89601e        3 weeks ago          /bin/sh -c #(nop)  CMD ["/bin/bash"]            0 B
<missing>           3 weeks ago          /bin/sh -c mkdir -p /run/systemd && echo '...   7 B
<missing>           3 weeks ago          /bin/sh -c sed -i 's/^#\s*\(deb.*universe\...   1.9 kB
<missing>           3 weeks ago          /bin/sh -c rm -rf /var/lib/apt/lists/*          0 B
<missing>           3 weeks ago          /bin/sh -c set -xe   && echo '#!/bin/sh' >...   745 B
<missing>           3 weeks ago          /bin/sh -c #(nop) ADD file:68f83d996c38a09...   129 MB</code>


Additional Dockerfile Instructions.

<code># Simple Webserver
FROM ubuntu:latest
RUN apt-get update
RUN apt-get install -y apache2
RUN apt-get install -y apache2-utils
RUN apt-get install -y vim
RUN apt-get clean
EXPOSE 80
CMD ["apache2ctl", "-D",  "FOREGROUND"]</code>
	
Can be reduced to:

<code># Simple Webserver
FROM ubuntu:latest
RUN apt-get update && apt-get install -y \
	apache2 \
	apache2-utils \
	vim \
	&& apt-get clean \
	&& rm -rf /var/tmp/*
	EXPOSE 80
	CMD ["apache2ctl", "-D",  "FOREGROUND"]</code>

This reduces the layers associated with the image being built.

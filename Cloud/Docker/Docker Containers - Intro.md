### Docker Containers

Docker containers are runtime versions of an image.
Some features of containers are:

Container runs in user space memory.

Bare minimum OS, Many packages missing.

<code># docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              67591570dd29        7 weeks ago         192 MB
fedora              latest              a1e614f0f30e        7 weeks ago         197 MB</code>

Running a container:

<code># docker run -it fedora /bin/bash
[root@1d99aad65e15 /]# 
Ctrl+P+Q to quit the container but not to exit
#</code>

<code># docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
1d99aad65e15        fedora              "/bin/bash"         11 seconds ago      Up 10 seconds                           dreamy_hypatia</code>


Separate dedicated filesystem for a container.
Separate dedicated memory stack for a container.
Control Groups (cgroups) used to set resource limits.
Capabilities break down root user access to allow the docker container have more finer grained access controls.

A container consists of all the layers used to create an image with a single writable layer on top.
The underlying read-only layers are shareable among containers.


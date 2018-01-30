You can find a PDF version of the sheet <a href="https://www.docker.com/sites/default/files/Docker_CheatSheet_08.09.2016.pdf">here</a>.

Of course you can also find more detailed info & full documentation at <a href="https://www.docker.com/">docker.io</a>


<strong>Working with a container.</strong>

docker run - Run a container

<code># docker run --rm --name container1 -it centos bash</code>

Options Used:

--rm - Remove the container when you exit
-it - Connect to the container and open a terminal
centos - Specify the image the container is instantiated from
bash - The command to run inside the container


docker stop - Stop a container TERM

<code># docker stop container1</code>


docker kill - Stop a container KILL

<code># docker kill container1</code>


docker network - Create / Manage a network

<code># docker network ls</code>

<code># docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 -d overlay mynet</code>


docker ps - See what docker containers are rinning / stopped etc.

<code># docker ps

CONTAINER ID        IMAGE                                                                   COMMAND                CREATED             STATUS              PORTS                    NAMES
4bf0a92c20d6        testserver01:5000/webserver:latest   "/startup.sh"          19 months ago       Up 19 months        0.0.0.0:7979->7979/tcp   webserver
8fe5f5aa0e5b        testserver01:5000/appserver:latest              "/tomcat/bin/catalin   20 months ago       Up 20 months        0.0.0.0:8080->8080/tcp   appserver</code>


docker rm - Delete a container. You can specify the container name or ID.

<code># docker rm container1</code>

<code># docker ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED           STATUS                     PORTS               NAMES
951af7f6c2fe        centos              "/bin/bash"         9 seconds ago       Exited (0) 7 seconds ago                       container1

# docker rm 951af7f6c2fe
951af7f6c2fe
#</code>


docker logs - View logs of a container

<code># docker logs container1</code>


docker exec - Execute a command in a container

<code># docker exec -it container1 bash</code>


<strong>Working with a images.</strong>


Build an image from a "Dockerfile" in the current directory

# docker build -t 

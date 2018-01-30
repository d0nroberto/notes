# Docker Container Restart Policies

The 'docker run' command is used to start a docker container.
Run time operands to 'docker run' allow you to specify a restart policy for 
a container if it is either stopped or killed.

~~~~
--restart = How a container should be restarted on exit.
~~~~

Values are:

~~~~
no = Don't restart the container (default value)

on-failure[:num] = Try restarting the container num times if the exit code is non-zero.

always = Continuously try to restart the container.

unless-stopped = Restart the container except when the docker daemon restarts and the container has been stopped.
~~~~

e.g.

~~~~
docker run --name=web_mysql -d -p 3306:3306 --restart=on-failure:10 -v /opt/mysql/conf.d:/etc/mysql/conf.d -v /opt/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password mysql/mysql
~~~~

If a container tries to restart and fails the delay between restarts doubles each time to stop the server being flooded.

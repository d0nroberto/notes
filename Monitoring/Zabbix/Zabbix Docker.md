#### Running Zabbix Using Public Containers

Assuming you have a host with docker engine installed...

You need to pull the following images from docker hub:

zabbix/zabbix-server-mysql
zabbix/zabbix-proxy-mysql
zabbix/zabbix-web-apache-mysql
mysql

~~~~
docker pull zabbix/zabbix-proxy-mysql

docker pull zabbix/zabbix-server-mysql

docker pull zabbix/zabbix-web-apache-mysql

docker pull mysql
~~~~

There is also a supported nginx image.

Use the following run syntax to run the server / database / web instances:

~~~~
docker run -d --name=zabbix_mysql -v /opt/docker/zabbix_mysql/var/lib/mysql:/var/lib/mysql -v /opt/docker/zabbix_mysql/etc/mysql/mysql.conf.d:/etc/mysql/mysql.conf.d -e MYSQL_ROOT_PASSWORD=changeme --restart=always -p 3306:3306 mysql

docker run -d --name zabbix_server_mysql --link=zabbix_mysql -e DB_SERVER_HOST="zabbix_mysql" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix" -e MYSQL_DATABASE="zabbix" zabbix/zabbix-server-mysql

docker run -d --name zabbix_webserver_mysql --link=zabbix_mysql --link=zabbix_server_mysql -e DB_SERVER_HOST="zabbix_mysql" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix" -e ZBX_SERVER_HOST="zabbix_server_mysql" -e PHP_TZ="Europe/Dublin" --restart=always -p 80:80 zabbix/zabbix-web-apache-mysql
~~~~

Further reading:

https://hub.docker.com/r/zabbix/zabbix-server-mysql/ https://hub.docker.com/r/zabbix/zabbix-proxy-mysql/ https://hub.docker.com/r/zabbix/zabbix-web-apache-mysql/

Consult these for details on the volumes you are allowed override & environment variables supported.

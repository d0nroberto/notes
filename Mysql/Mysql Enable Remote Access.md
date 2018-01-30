### Enabling Remote Access to Mysql Server

#### Configuring the mysql server

You must edit the /etc/my.cnf file to enable the server to be accessible from a remote IP address.
Comment out the 'skip-networking' variable and configure the 'bind-address' variable.

~~~~

# skip-networking
bind-address=[IP_ADDRESS]

~~~~

Restart the mysql server for the configuration change to take effect.

#### Configuring the database

You can now grant access to a database for a specific user coming from a specific IP address.
Login to the database server and use the 'grant' SQL command.

~~~~

mysql> grant all on mydatabase.* TO john_doe@'192.168.1.15' IDENTIFIED BY 'password';

~~~~

To Update access:

~~~~

mysql> update db set Host='192.168.1.25' where Db='mydatabase';
mysql> update user set Host='192.168.1.25' where user='john_doe';

~~~~

You may also need to consider firewalld / selinux if they are running on your host.

# Zabbix Agent Manual Installation

The Zabbix server is the software installed in a host run the zabbix server process.
It also requires a back-end database instance e.g. mysql / mariadb.

### 1. Install Packages

You can download the rpm files for the individual services from [here](http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/)

To install the repos:

~~~~
# rpm -ivh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1.el7.noarch.rpm
# yum repolist
.....
.....
zabbix/x86_64               Zabbix Official Repository - x86_64             105
zabbix-non-supported/x86_64 Zabbix Official Repository non-supported - x8     4
~~~~

Installation of server packages:

~~~~
# yum install zabbix-server-mysql zabbix-web-mysql

....
....
 mailcap              noarch  2.1.41-2.el7          base                   31 k
 net-snmp-libs        x86_64  1:5.7.2-24.el7_2.1    base                  747 k
 php                  x86_64  5.4.16-42.el7         base                  1.4 M
 php-bcmath           x86_64  5.4.16-42.el7         base                   57 k
 php-cli              x86_64  5.4.16-42.el7         base                  2.7 M
 php-common           x86_64  5.4.16-42.el7         base                  564 k
 php-gd               x86_64  5.4.16-42.el7         base                  127 k
 php-ldap             x86_64  5.4.16-42.el7         base                   52 k
 php-mbstring         x86_64  5.4.16-42.el7         base                  505 k
 php-mysql            x86_64  5.4.16-42.el7         base                  101 k
 php-pdo              x86_64  5.4.16-42.el7         base                   98 k
 php-xml              x86_64  5.4.16-42.el7         base                  125 k
 t1lib                x86_64  5.1.2-14.el7          base                  166 k
 unixODBC             x86_64  2.3.1-11.el7          base                  413 k
 zabbix-web           noarch  3.0.7-1.el7           zabbix                3.5 M

Transaction Summary
================================================================================
Install  2 Packages (+29 Dependent packages)

# id zabbix
uid=997(zabbix) gid=995(zabbix) groups=995(zabbix)
~~~~

### 2. Setup The Database

Installation of mysql mariaDB:

~~~~
# yum install mariadb mariadb-server

# systemctl enable mariadb
Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
# systemctl start mariadb
# mysql_secure_installation
~~~~

Setup Database Users etc.:

~~~~
MariaDB [(none)]> create database zabbix character set utf8 collate utf8_bin;

MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost identified by '<password>';
~~~~

To create the user account in mysql.user...

~~~~
MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO 'root'@'10.0.197.%' IDENTIFIED BY 'secret_password' WITH GRANT OPTION;

MariaDB [mysql]> flush privileges;
~~~~

To install the database:

~~~~
# cd /usr/share/doc/zabbix-server-mysql-*

# zcat create.sql.gz | mysql -u zabbix -D zabbix -p
Enter password:
~~~~

If you want to be able to remotely access the database, follow these steps:

Assuming you are running firewalld...

~~~~
# firewall-cmd --add-port=3306/tcp
success
# firewall-cmd --permanent --add-port=3306/tcp
success
#
~~~~

### 3. Setup Zabbix Server Config

Now edit the file /etc/zabbix/zabbix_server.conf file specifying the database parameters.

~~~~
       DBHost=localhost
       DBName=zabbix
       DBUser=zabbix
       DBPassword=zabbix
       DBPort=3306
~~~~

The package zabbix-web-mysql installs an apache httpd instance.
Also edit the php config parameter for the current timezone in /etc/httpd/conf.d/zabbix.conf

~~~~
       php_value date.timezone Europe/Riga
~~~~

If you have selinux enabled...

~~~~
# setsebool -P httpd_can_connect_zabbix on
# setsebool -P zabbix_can_network on
~~~~

Zabbix disables core dumps on UNIX platforms if compiled with encryption and does not start if system (e.g. SELinux policy) does not allow disabling of core dumps.

Checking /var/log/audit/audit.log:

~~~~
# grep zabbix /var/log/audit/audit.log | grep denied | more
....
for  pid=21362 comm="zabbix_server" scontext=system_u:system_r:zabbix_t:s0 tcontext=system_u:system_r:zabbix_t:s0 tclass=process type=AVC msg=audit(1484664060.780:496): avc:  denied  for  pid=21367 comm="zabbix_server" scontext=system_u:system_r:zabbix_t:s0 tcontext=system_u:system_r:zabbix_t:s0 tclass=processtype=AVC msg=audit(1484664071.029:500): avc:  denied  for  pid=21373 comm="zabbix_server" scontext=system_u:system_r:zabbix_t:s0 tcontext=system_u:system_r:zabbix_t:s0 tclass=processtype=AVC msg=audit(1484664081.280:504): avc:  denied  ....

# semanage permissive -a zabbix_t
~~~~

And also if you have firewalld running...

~~~~
# firewall-cmd --add-port=80/tcp
# firewall-cmd --permanent --add-port=80/tcp

# firewall-cmd --add-port=10050/tcp
# firewall-cmd --permanent --add-port=10050/tcp
# firewall-cmd --add-port=10050/udp
# firewall-cmd --permanent --add-port=10050/udp

# firewall-cmd --add-port=10051/tcp
# firewall-cmd --permanent --add-port=10051/tcp
# firewall-cmd --add-port=10051/udp
# firewall-cmd --permanent --add-port=10051/udp
~~~~

### 4. Enable / start services

Finally enable and start both the httpd and zabbix services.

~~~~
# systemctl enable zabbix-server
# systemctl start zabbix-server
# systemctl enable httpd
# systemctl start httpd
~~~~

You should now have access to the front end using http://localhost/zabbix
The default login is *Admin / zabbix*. This is the zabbix 'superuser' account.

### 5. Complete Frontend Configuration

Navigate to http://localhost/zabbix

First a prerequisite check is carried out.

~~~~
Check of pre-requisites
Current value    Required    
PHP version    5.4.16    5.4.0    OK
PHP option "memory_limit"    128M    128M    OK
PHP option "post_max_size"    16M    16M    OK
PHP option "upload_max_filesize"    2M    2M    OK
PHP option "max_execution_time"    300    300    OK
PHP option "max_input_time"    300    300    OK
PHP option "date.timezone"    Europe/Riga        OK
PHP databases support    MySQL        OK
PHP bcmath    on        OK
PHP mbstring    on        OK
PHP option "mbstring.func_overload"    off    off    OK
PHP sockets    on        OK
PHP gd    2.1.0    2.0    OK
PHP gd PNG support    on        OK
PHP gd JPEG support    on        OK
PHP gd FreeType support    on        OK
PHP libxml    2.9.1    2.6.15    OK
PHP xmlwriter    on        OK
PHP xmlreader    on        OK
PHP ctype    on        OK
PHP session    on        OK
PHP option "session.auto_start"    off    off    OK
PHP gettext    on        OK
PHP option "arg_separator.output"    &    &    OK
~~~~

Next you are asked for database login details. Provide those set earlier.

Finally a congratulations screen followed by the login dialog.

~~~~
    Congratulations! You have successfully installed Zabbix frontend.
    Configuration file "/etc/zabbix/web/zabbix.conf.php" created.
~~~~

You can now login with the Admin credentials.

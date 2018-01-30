### Zabbix Web Installation - Manual Steps

#### Install Zabbix Packages

~~~~
# wget http://repo.zabbix.com/zabbix/3.2/rhel/7/x86_64/zabbix-release-3.2-1.el7.noarch.rpm
# rpm -ivf zabbix-release-3.2-1.el7.noarch.rpm
# yum install zabbix-web-mysql

# yum install httpd
# systemctl enable httpd
# systemctl start httpd
~~~~

#### Configure SELinux (If in use)

~~~~
# sestatus
# setsebool -P httpd_can_network_connect_db=1
# getsebool -a| grep zabbix
# setsebool -P zabbix_can_network=1
# setsebool -P httpd_can_connect_zabbix=1
# getsebool -a| grep zabbix

# semanage port -a -t http_port_t -p tcp 10051
# semanage port -m -t http_port_t -p tcp 10051
~~~~

#### Complete Installation

Provide the config options needed by /etc/zabbix/web/zabbix.conf.php at http://[host_fqdn]/zabbix

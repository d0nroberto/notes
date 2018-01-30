# Zabbix Agent Manual Installation

The Zabbix agent is the agent software installed in a host monitored by Zabbix.

### 1. Install Packages

You can download the rpm files for the individual services from [here](http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/)

To install the repos:

~~~~
# rpm -ivh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1.el7.noarch.rpm
# yum repolist
.....
zabbix/x86_64               Zabbix Official Repository - x86_64             105
zabbix-non-supported/x86_64 Zabbix Official Repository non-supported - x8     4
.....
~~~~

Installation of the agent package:

~~~~
# yum install zabbix-agent
~~~~

### 2. Setup Zabbix Server Config

Now edit the file /etc/zabbix/zabbix_agent.conf file specifying the agent parameters.

~~~~
       EnableRemoteCommands=1
       LogRemoteCommands=1
       Server=[zabbix server]
       ServerActive=[zabbix server]
       Hostname=[zabbix host]
       Include=/etc/zabbix/zabbix_agentd.d/
~~~~

Optional Config e.g. /etc/zabbix/zabbix_agent.d/UserParams.conf:

~~~~
UserParameter=kernel.shmall,sysctl -a | grep kernel.shmall | cut -d ' ' -f3
UserParameter=kernel.shmmax,sysctl -a | grep kernel.shmmax | cut -d ' ' -f3
UserParameter=kernel.shmmni,sysctl -a | grep kernel.shmmni | cut -d ' ' -f3
#File count of a directory
UserParameter=vfs.dir.filecount[*],find $1 -maxdepth 1 -type f | wc -l
~~~~

If you have selinux enabled...

~~~~
# setsebool \-P httpd_can_connect_zabbix on
# setsebool \-P zabbix_can_network on
~~~~

Checking /var/log/audit/audit.log:

~~~~
# grep zabbix /var/log/audit/audit.log | grep denied | more
....
type=AVC msg=audit(1488287256.790:34379): avc: denied { setrlimit } for pid=10064 comm="zabbix_agentd"  scontext=system_u:system_r:zabbix_agent_t:s0  tcontext=system_u:system_r:zabbix_agent_t:s0 tclass=process
type=AVC msg=audit(1488287267.028:34384): avc: denied { setrlimit } for pid=10072 comm="zabbix_agentd"  scontext=system_u:system_r:zabbix_agent_t:s0  tcontext=system_u:system_r:zabbix_agent_t:s0 tclass=process
....

# semanage permissive -a zabbix_agent_t
~~~~

Also if you have firewalld running...

~~~~
# firewall-cmd --add-port=10050/tcp
# firewall-cmd --permanent --add-port=10050/tcp
# firewall-cmd --add-port=10050/udp
# firewall-cmd --permanent --add-port=10050/udp
~~~~

### 3. Enable / start services

Finally enable and start both the httpd and zabbix services.

~~~~
# systemctl enable zabbix-agent
# systemctl start zabbix-agent
~~~~

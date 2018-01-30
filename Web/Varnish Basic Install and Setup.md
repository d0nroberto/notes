# Varnish Basic Installation and Daemon Setup

Varnish provides a cache layer fronting a webserver.

The basic package installation & daemon configuration parameters are described below.

Based on a RHEL host.


### Package installation:

~~~~
# yum -y install varnish


Dependency Installed:
  dwz.x86_64 0:0.11-3.el7
  jemalloc.x86_64 0:3.6.0-1.el7
  perl-srpm-macros.noarch 0:1-8.el7
  redhat-rpm-config.noarch 0:9.1.0-72.el7.centos
  varnish-libs.x86_64 0:4.0.4-3.el7
  zip.x86_64 0:3.0-11.el7
~~~~

The folder containing config files is /etc/varnish.
The varnish caching rules  are stored in a'vcl' file: /etc/varnish/default.vcl


### Daemons Installed

Installation creates these services

~~~~
# systemctl list-unit-files | grep varnish

varnish.service                               disabled
varnishlog.service                            disabled
varnishncsa.service                           disabled
~~~~

Manualy enable the ones you want to run.

~~~~
# systemctl enable varnish
Created symlink from /etc/systemd/system/multi-user.target.wants/varnish.service to /usr/lib/systemd/system/varnish.service.

# systemctl enable varnishncsa
Created symlink from /etc/systemd/system/multi-user.target.wants/varnishncsa.service to /usr/lib/systemd/system/varnishncsa.service.
~~~~


#### varnishlog - Varnish logging daemon

Startup syntax:

~~~~
ExecStart=/usr/bin/varnishlog -a -w /var/log/varnish/varnish.log -D -P /run/varnishlog/varnishlog.pid

-a = append log
-w = write to file
-D = run as daemon
-P = Pid file
~~~~


#### varnishncsa - Varnish NCSA logging daemon (Display varnish logs in Apache format)

Startup syntax:

~~~~
ExecStart=/usr/bin/varnishncsa -a -w /var/log/varnish/varnishncsa.log -D -P /run/varnishncsa/varnishncsa.pid

-a = append log
-w = write to file
-D = run as daemon
-P = Pid file

-F = format
~~~~

The default log format is:          %h %l %u %t "%r" %s %b "%{Referer}i" "%{User-agent}i"


#### varnishd - HTTP caching daemon

Startup syntax:

~~~~
EnvironmentFile=/etc/varnish/varnish.params

ExecStart=/usr/sbin/varnishd \
        -P /var/run/varnish.pid \
        -f $VARNISH_VCL_CONF \
        -a ${VARNISH_LISTEN_ADDRESS}:${VARNISH_LISTEN_PORT} \
        -T ${VARNISH_ADMIN_LISTEN_ADDRESS}:${VARNISH_ADMIN_LISTEN_PORT} \
        -S $VARNISH_SECRET_FILE \
        -s $VARNISH_STORAGE \
        $DAEMON_OPTS

-a address[:port][,address[:port][...] = Listen for client requests on the specified address and port

-b host[:port] = Use the specified host as backend server. If port is not specified, the default is 8080.

-M address:port =  Connect to this port and offer the command  line  interface. Think  of  it as a reverse shell.

-T address[:port] = Offer a management interface on the specified address and port.

-S file = Path to a file containing a secret used for authorizing access to the management port.

-d = Enables debugging mode

-f config = Use the specified VCL configuration file instead of the  builtin default.

-F = Run in the foreground.

-n name = Specify  the  name for this instance. If the name begins with a slash, it is interpreted as
              the path to the directory which should be used to store temporary files.

-P file = Write the process's PID to the specified file.

-u user = The name of an unprivileged user to  which  the  child process  should  switch  before it starts accepting connections.

-g group = The name of an unprivileged group to which  the  child process  should  switch  before it starts accepting connections

-s [name=]type[,options] = Use  the  specified storage backend. The storage backends can be one of the following:

                     · malloc[,size]
                     · file[,path[,size[,granularity]]]
                     · persistent,path,size

-t ttl = A hard minimum time to live for cached documents.
~~~~

e.g.:

~~~~
varnishd -a 0.0.0.0:1025 -f /home/varnish01/etc/default.vcl -s malloc,1G -n /home/varnish01/log -P /home/varnish01/pid/varnish.pid
~~~~

Edit the file /etc/varnish/varnish.params with the CLI options you want to use e.g.

~~~~
# cat /etc/varnish/varnish.params
# Varnish environment configuration description. This was derived from
# the old style sysconfig/defaults settings

# Set this to 1 to make systemd reload try to switch VCL without restart.
RELOAD_VCL=1

# Main configuration file. You probably want to change it.
VARNISH_VCL_CONF=/etc/varnish/www.vcl

# Default address and port to bind to. Blank address means all IPv4
# and IPv6 interfaces, otherwise specify a host name, an IPv4 dotted
# quad, or an IPv6 address in brackets.
#VARNISH_LISTEN_ADDRESS=127.0.0.1
VARNISH_LISTEN_PORT=80

# Admin interface listen address and port
VARNISH_ADMIN_LISTEN_ADDRESS=127.0.0.1
VARNISH_ADMIN_LISTEN_PORT=6082

# Shared secret file for admin interface
VARNISH_SECRET_FILE=/etc/varnish/secret

# Backend storage specification, see Storage Types in the varnishd(5)
# man page for details.
VARNISH_STORAGE="malloc,256M"

# User and group for the varnishd worker processes
VARNISH_USER=varnish
VARNISH_GROUP=varnish

# Other options, see the man page varnishd(1)
DAEMON_OPTS="-p thread_pool_min=5 -p thread_pool_max=500 -p thread_pool_timeout=300"

~~~~


Once you have the options configured as you wish, start the daemons you wish to run.

~~~~

# systemctl start varnish

# systemctl start varnishncsa

~~~~

Note the above example will try to start a varnish daemon instance on port 80.
If you already have a service bound to this port e.g. httpd, it will fail to start:

~~~~
Apr 05 10:34:30 systemd[1]: varnish.service: control process exite
Apr 05 10:34:30 systemd[1]: Failed to start Varnish Cache, a high-
-- Subject: Unit varnish.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit varnish.service has failed.
--
-- The result is failed.
Apr 05 10:34:30 systemd[1]: Unit varnish.service entered failed st
Apr 05 10:34:30 systemd[1]: varnish.service failed.

# lsof -i:80
COMMAND   PID  USER   FD   TYPE   DEVICE SIZE/OFF NODE NAME
nginx   11053  root    7u  IPv4 15013245      0t0  TCP *:http (LISTEN)
nginx   11054 nginx    7u  IPv4 15013245      0t0  TCP *:http (LISTEN)
~~~~

In this example the listen port used by nginx must be moved off of port 80.

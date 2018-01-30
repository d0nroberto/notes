# HAProxy Setup Guide

### Intro

HAProxy is an open source load balancer for distributing load across multiple servers.

### Installation


On RHEL based systems using yum:

~~~~

# yum -y install haproxy
...
Installed:
  haproxy.x86_64 0:1.5.18-3.el7_3.1

~~~~

This installs a systemd service haproxy.service which is disabled by default.

Enable and start the service as follows:

~~~~

# systemctl enable haproxy.service
Created symlink from /etc/systemd/system/multi-user.target.wants/haproxy.service to /usr/lib/systemd/system/haproxy.service.

# systemctl start haproxy.service
# ps -ef |grep haproxy
root      4939     1  0 17:36 ?        00:00:00 /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
haproxy   4940  4939  0 17:36 ?        00:00:00 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
haproxy   4941  4940  0 17:36 ?        00:00:00 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds

~~~~

### Configuration

Systemd maintains the service startup. The unit file can be found here:

~~~~

#  more /usr/lib/systemd/system/haproxy.service
[Unit]
Description=HAProxy Load Balancer
After=syslog.target network.target

[Service]
EnvironmentFile=/etc/sysconfig/haproxy
ExecStart=/usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid $OPTIONS
ExecReload=/bin/kill -USR2 $MAINPID

[Install]
WantedBy=multi-user.target

~~~~

An environment file /etc/sysconfig/haproxy can be used to specify the startup options used by the OPTIONS variable.

OPTIONS available (from the man page) - 

OPTIONS
       -f <configuration file> = Specify configuration file path.
       -L <name> = Set the local instance's peer name. 
       -n <maxconn> = Set the high limit for the total number of simultaneous connections.
       -N <maxconn> = Set the high limit for the per-listener number of simultaneous connections.
       -C <dir> = Change directory to <dir> before loading any files.
       -v = Display HAProxy's version.
       -vv = Display HAProxy's version and all build options.
       -d = Start in foreground with debugging mode enabled.  
       -D = Start in daemon mode.
       -Ds = Start in systemd daemon mode, keeping a process in foreground.
       -q = Disable messages on output.
       -V = Displays messages on output even when -q or 'quiet' are specified.
       -c = Only checks config file and exits with code 0 if no error was found, or exits with code 1 if a syntax error was found.
       -p <pidfile> = Ask the process to write down each of its children's pids to this file in daemon mode.
       -dk = Disable use of kqueue(2). kqueue(2) is available only on BSD systems.
       -ds = Disable use of speculative epoll(7). epoll(7) is available only on Linux 2.6 and some custom Linux 2.4 systems.
       -de = Disable use of epoll(7). epoll(7) is available only on Linux 2.6 and some custom Linux 2.4 systems.
       -dp = Disables use of poll(2). select(2) might be used instead.
       -dS = Disables use of splice(2), which is broken on older kernels.
       -db = Disables background mode (stays in foreground, useful for debugging). 
       -dM[<byte>] = Initializes  all  allocated  memory  areas with the given <byte>. 
       -m <megs> = Enforce a memory usage limit to a maximum of <megs> megabytes.
       -sf <pidlist> = Send FINISH signal to the pids in pidlist after startup.
       -st <pidlist> = Send TERMINATE signal to the pids in pidlist after startup.


Default config server file haproxy.cfg can be found in /etc/haproxy folder.

This config file contains 3 main sections:

**global** - Global settings for the daemon e.g. pid file, user the daemon runs as etc.

**frontend** - Proxy Section. What front ends the HAPorxy daemon provides.

**backend** - Proxy Section. What back end servers the HaProxy daemon routes traffic to.

**listen** - Proxy Section. Both backend and frontend configs combined in the proxy configuration.


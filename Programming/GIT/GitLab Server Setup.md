# GitLab Setup

<a href="https://about.gitlab.com/">GitLab</a>



GitLab provides a web frontend for managing your git projects.
The free version with limited features is known as the Community Edition (CE)

You can find a full comparison between versions & pricing <a href="https://about.gitlab.com/products/">here</a>



### Installation


GitLab uses Omnibus packages - <a href="https://github.com/chef/omnibus/blob/master/README.md">https://github.com/chef/omnibus/blob/master/README.md</a>


Find the specific instructions for your OS here <a href="https://about.gitlab.com/downloads/">https://about.gitlab.com/downloads/</a>



You can either:
  Manually install the GitLab package
or 
  Run a script to setup the correct repo & install using yum.

Focusing on RHEL...

### Manually downloading and installing the package.


You can find the package here <a href="https://packages.gitlab.com/gitlab/gitlab-ce">https://packages.gitlab.com/gitlab/gitlab-ce</a>

 

Using a link you generate you can wget / curl the file down to your local system.
You should then be able to use ~~~rpm -ivf~~~ to install the package.


### Running the script to setup the repo etc.


~~~
  # wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh

  # chmod 755 script.rpm.sh
  # ./script.rpm.sh
~~~

The script will create the ~~~ /etc/yum.repos.d/gitlab_gitlab-ce.repo ~~~ file etc. which will provide the gitlab package.

Now to install GitLab using yum:

~~~ ### yum install gitlab-ce ~~~


After the package installation completes, browse to your host & configure a default password for your initial user.

Default initial user is ~~~ 'root' ~~~ which you can also change.

Some of the steps carried out by the installer script:

~~~  Installed: gitlab-ce-8.14.4-ce.0.el7.x86_64

 group added to /etc/group: name=gitlab-www

 new user: name=gitlab-www home=/var/opt/gitlab/nginx, shell=/bin/false

 change user 'git' home from '/home/git' to '/var/opt/gitlab'

 group added to /etc/group: name=gitlab-redis

 new user: name=gitlab-redis, UID=984, GID=979, home=/var/opt/gitlab/redis, shell=/bin/false
 group added to /etc/group: name=gitlab-psql
 new user: name=gitlab-psql home=/var/opt/gitlab/postgresql, shell=/bin/sh ~~~



### Starting / Stopping the service.

GitLab should start by default after installation.

You use the ~~~ gitlab-ctl ~~~ command to manage the GitLab service(s).


~~~ 
# gitlab-ctl status
run: gitlab-workhorse: (pid 20887) 107425s; run: log: (pid 20539) 107499s
run: logrotate: (pid 17444) 3078s; run: log: (pid 20652) 107482s
run: nginx: (pid 20593) 107488s; run: log: (pid 20592) 107488s
run: postgresql: (pid 20142) 107594s; run: log: (pid 20141) 107594s
run: redis: (pid 19987) 107610s; run: log: (pid 19986) 107610s
run: sidekiq: (pid 20490) 107511s; run: log: (pid 20489) 107511s
run: unicorn: (pid 20428) 107517s; run: log: (pid 20427) 107517s ~~~

You can also use the systemd service ~~~ gitlab-runsvdir ~~~

~~~ 

# systemctl status gitlab-runsvdir
 gitlab-runsvdir.service - GitLab Runit supervision process
   Loaded: loaded (/usr/lib/systemd/system/gitlab-runsvdir.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2016-12-14 11:17:32 GMT; 2 days ago
 Main PID: 19927 (runsvdir)
   CGroup: /system.slice/gitlab-runsvdir.service
           16083 postgres: gitlab gitlabhq_production [local] idle
           17087 postgres: gitlab gitlabhq_production [local] idle
           18035 /bin/sh /opt/gitlab/embedded/bin/gitlab-logrotate-wrapper
           18040 sleep 600
           18769 sleep 1
           19927 runsvdir -P /opt/gitlab/service log: ........................
           19985 runsv redis
           19986 svlogd -tt /var/log/gitlab/redis
           19987 /opt/gitlab/embedded/bin/redis-server 127.0.0.1:0
           20140 runsv postgresql
           20141 svlogd -tt /var/log/gitlab/postgresql
           20142 /opt/gitlab/embedded/bin/postgres -D /var/opt/gitlab/postg...
           20144 postgres: checkpointer process
           20145 postgres: writer process
           20146 postgres: wal writer process
           20147 postgres: autovacuum launcher process
           20148 postgres: stats collector process
           20426 runsv unicorn
           20427 svlogd -tt /var/log/gitlab/unicorn
           20428 /bin/bash /opt/gitlab/embedded/bin/gitlab-unicorn-wrapper
           20451 unicorn master -D -E production -c /var/opt/gitlab/gitlab-...
           20488 runsv sidekiq
           20489 svlogd -tt /var/log/gitlab/sidekiq
           20490 sidekiq 4.2.1 gitlab-rails [0 of 25 busy]
           20537 runsv gitlab-workhorse
           20539 svlogd -tt /var/log/gitlab/gitlab-workhorse
           20548 postgres: gitlab gitlabhq_production [local] idle
           20591 runsv nginx
           20592 svlogd -tt /var/log/gitlab/nginx
           20593 nginx: master process /opt/gitlab/embedded/sbin/nginx -p /...
           20594 nginx: worker process
           20595 nginx: worker process
           20596 nginx: cache manager process
           20651 runsv logrotate
           20652 svlogd -tt /var/log/gitlab/logrotate
           20677 postgres: gitlab gitlabhq_production [local] idle
           20792 unicorn worker[0] -D -E production -c /var/opt/gitlab/gitl...
           20795 unicorn worker[1] -D -E production -c /var/opt/gitlab/gitl...
           20887 /opt/gitlab/embedded/bin/gitlab-workhorse -listenNetwork u...
           20903 postgres: gitlab gitlabhq_production [local] idle
           21024 postgres: gitlab gitlabhq_production [local] idle
           30818 postgres: gitlab gitlabhq_production [local] idle 
The systemd wrapper for GitLab calls this service - /opt/gitlab/embedded/bin/runsvdir-start


###GitLab Service Configuration

The file ~~~ /etc/gitlab/gitlab.rb ~~~ configures the GitLab instance.
You may need to run ~~~ gitlab-ctl reconfigure ~~~ after making changes.



### Log files...

You can find the many log files associated with GitLab and all it's bits & pieces under ~~~ /var/log/gitlab/ ~~~

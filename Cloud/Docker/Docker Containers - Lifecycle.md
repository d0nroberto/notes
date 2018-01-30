### Starting, Stopping & Deleting Containers

You start a container with the docker-run command.

<code># docker run</code>

There are many options to the docker run command. For example:

<code>-d = Run a container in detached mode
-P = Expose all container ports
-i = Interactive mode</code>

Look at the docker-run manpage to get details on all options.

<code>docker run [-a|--attach[=[]]] [--add-host[=[]]] [--blkio-weight[=[BLKIO-WEIGHT]]] [--blkio-weight-device[=[]]] [--cpu-shares[=0]] [--cap-add[=[]]] [--cap-drop[=[]]]
[--cgroup-parent[=CGROUP-PATH]] [--cidfile[=CIDFILE]] [--cpu-count[=0]] [--cpu-percent[=0]] [--cpu-period[=0]] [--cpu-quota[=0]] [--cpu-rt-period[=0]] [--cpu-rt-runtime[=0]] [--cpus[=0.0]]
[--cpuset-cpus[=CPUSET-CPUS]] [--cpuset-mems[=CPUSET-MEMS]] [-d|--detach] [--detach-keys[=[]]] [--device[=[]]] [--device-read-bps[=[]]] [--device-read-iops[=[]]] [--device-write-bps[=[]]]
[--device-write-iops[=[]]] [--dns[=[]]] [--dns-option[=[]]] [--dns-search[=[]]] [-e|--env[=[]]] [--entrypoint[=ENTRYPOINT]] [--env-file[=[]]] [--expose[=[]]] [--group-add[=[]]]
[-h|--hostname[=HOSTNAME]] [--help] [-i|--interactive] [--ip[=IPv4-ADDRESS]] [--ip6[=IPv6-ADDRESS]] [--ipc[=IPC]] [--isolation[=default]] [--kernel-memory[=KERNEL-MEMORY]] [-l|--label[=[]]]
[--label-file[=[]]] [--link[=[]]] [--link-local-ip[=[]]] [--log-driver[=[]]] [--log-opt[=[]]] [-m|--memory[=MEMORY]] [--mac-address[=MAC-ADDRESS]] [--memory-reservation[=MEMORY-RESERVATION]]
[--memory-swap[=LIMIT]] [--memory-swappiness[=MEMORY-SWAPPINESS]] [--name[=NAME]] [--network-alias[=[]]] [--network[="bridge"]] [--oom-kill-disable] [--oom-score-adj[=0]] [-P|--publish-all]
[-p|--publish[=[]]] [--pid[=[PID]]] [--userns[=[]]] [--pids-limit[=PIDS_LIMIT]] [--privileged] [--read-only] [--restart[=RESTART]] [--rm] [--security-opt[=[]]] [--storage-opt[=[]]]
[--stop-signal[=SIGNAL]] [--stop-timeout[=TIMEOUT]] [--shm-size[=[]]] [--sig-proxy[=true]] [--sysctl[=[]]] [-t|--tty] [--tmpfs[=[CONTAINER-DIR[:<OPTIONS>]]] [-u|--user[=USER]] [--ulimit[=[]]]
[--uts[=[]]] [-v|--volume[=[[HOST-DIR:]CONTAINER-DIR[:OPTIONS]]]] [--volume-driver[=DRIVER]] [--volumes-from[=[]]] [-w|--workdir[=WORKDIR]] IMAGE [COMMAND] [ARG...]</code>


# Starting in interactive mode

<code># docker run -it ubuntu /bin/bash</code>

To stop an interactive container exit it.
	
<code>root@fcd4ab29cdbe:/# exit
exit
#</code>
		
To quit an interactive session and leave the container running type Ctrl+P+Q


# Stopping a container:

To stop this container use docker stop or docker kill.
These send a signal to the process inside the container with a PID of 1

<code># docker stop</code>

<code># docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
0cb35f346277        webserver:0.1       "apache2ctl -D FOR..."   3 minutes ago       Exited (137) 5 seconds ago                           blissful_goldwasser

# docker restart 0cb35f346277
0cb35f346277


# Hard-stop a container

<code># docker kill</code>

This hard stops a container.

<code># docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
3d0ab7794d99        ubuntu              "/bin/bash"         11 seconds ago      Up 10 seconds                           determined_galileo

# docker kill determined_galileo
determined_galileo

# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES</code>


# Restarting a container

<code># docker restart</code>

<code># docker stop 0cb35f346277
0cb35f346277

# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
0cb35f346277        webserver:0.1       "apache2ctl -D FOR..."   3 minutes ago       Exited (137) 5 seconds ago                           blissful_goldwasser

# docker restart 0cb35f346277
0cb35f346277

# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
0cb35f346277        webserver:0.1       "apache2ctl -D FOR..."   3 minutes ago       Up 2 seconds        0.0.0.0:80->80/tcp   blissful_goldwasser</code>


# Deleting a container

Use the docker-rm command to remove a container.

<code># docker info| more
Containers: 11
 Running: 0
 Paused: 0
 Stopped: 11
Images: 20

# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
3d0ab7794d99        ubuntu              "/bin/bash"              2 hours ago         Exited (137) 2 hours ago                       determined_galileo
b92773b8bef0        ubuntu              "/bin/bash"              2 hours ago         Exited (0) 2 hours ago                         nifty_hoover
268ecc091616        ubuntu              "/bin/bash -c 'pin..."   2 hours ago         Exited (127) 2 hours ago                       tender_poincare
6a480a4c483c        ubuntu              "/bin/bash"              2 hours ago         Exited (0) 2 hours ago                         adoring_varahamihira
06b6cb9a4f7e        ubuntu              "/bin/bash"              2 hours ago         Exited (0) 2 hours ago                         boring_swirles
fcd4ab29cdbe        ubuntu              "/bin/bash"              2 hours ago         Exited (0) 2 hours ago                         epic_babbage
78cb38674975        centos              "/bin/bash -c 'ech..."   25 hours ago        Exited (0) 25 hours ago                        epic_almeida
a638429d20a5        centos              "/bin/bash -c 'ech..."   25 hours ago        Exited (0) 25 hours ago                        musing_lewin
21c76a818b95        centos              "/bin/bash -c 'ech..."   25 hours ago        Exited (0) 25 hours ago                        goofy_blackwell
a1c95742b3ce        hello-world         "/hello"                 2 days ago          Exited (0) 2 days ago                          dreamy_bassi
86c5cddf0a90        hello-world         "/hello"                 5 days ago          Exited (0) 5 days ago                          kickass_aryabhata

# docker rm 3d0ab7794d99
3d0ab7794d99</code>

This will remove the container, however the base images may still be present on the system.

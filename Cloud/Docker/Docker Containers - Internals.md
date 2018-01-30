### Inspecting the internal processes running in a Container

Use the docker-ps and docker-top commands to see what process is running in a container:

<code># docker run -d motd_busted /bin/bash -c "ping 8.8.8.8"
2327c5ed747b3376456d0f305b32ef7f512209aad98f0902706ae95eca203018

# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
2327c5ed747b        motd_busted         "/bin/bash -c 'pin..."   4 seconds ago       Up 3 seconds                            thirsty_curran

# docker top 2327c5ed747b
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                9044                9027                0                   14:29               ?                   00:00:00            ping 8.8.8.8

# ps -ef | grep 9044
root      9044  9027  0 14:29 ?        00:00:00 ping 8.8.8.8</code>

All STDOUT and STDERR produced by a command is captured by docker logs.
To see the output of a container use docker-logs:

<code># docker logs 2327c5ed747b
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=51 time=19.0 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=51 time=17.5 ms</code>

To tail the output of a container

<code># docker logs --follow 2327c5ed747b 
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=51 time=19.0 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=51 time=17.5 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=51 time=16.4 ms</code>

You can also use the docker-inspect command to see what command a process runs on start-up.


<code># docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS               NAMES
ba8e6d675f29        motd_busted         "/bin/bash -c 'pin..."   About a minute ago   Up About a minute                       eager_goldwasser

# docker inspect ba8e6d675f29 | more

[
    {
        "Id": "ba8e6d675f29774db56bc42e4a8764570354eb4b3f6a89550c4e3513f4e86725",
        "Created": "2017-02-09T14:43:44.187653681Z",
        "Path": "/bin/bash",
        "Args": [
            "-c",
            "ping 8.8.8.8"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 9536,
            "ExitCode": 0,
…..

        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "74e8685618782bec3d8e6c0cf3ff9c517cdc57cd5c379cd5892a36ca32ca4480",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/74e868561878",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "a2e5ec903244e38e942bb42933052d9d0ea09a366ba1857298169480e3940e13",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
…..</code>
	
To gain access to a container:

If you run docker attach to attach to a container, you will be attached to the container PID 1, not an interactive shell.

To gain a shell on a container, use docker exec

<code># docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
ba8e6d675f29        motd_busted         "/bin/bash -c 'pin..."   14 minutes ago      Up 14 minutes                           eager_goldwasser
# docker exec -it ba8e6d675f29 /bin/bash
[root@ba8e6d675f29 /]# top
top - 14:58:23 up 5 days, 23:37,  0 users,  load average: 0.01, 0.04, 0.05
Tasks:   3 total,   1 running,   2 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.3 sy,  0.0 ni, 99.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1883996 total,   105692 free,   218596 used,  1559708 buff/cache
KiB Swap:  2097148 total,  2097040 free,      108 used.  1298196 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0   24324   1212    892 S   0.0  0.1   0:00.38 ping
   29 root      20   0   11768   1884   1504 S   0.0  0.1   0:00.08 bash
   41 root      20   0   51892   1860   1388 R   0.0  0.1   0:00.04 top
</code>

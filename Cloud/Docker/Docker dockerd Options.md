### Specify Startup Options for the Docker Daemon

On RHEL 7 systems, docker is managed by systemd.
The service unit file can be found at '/usr/lib/systemd/system/docker.service'
However this file should not be modified.

The recommended way to alter startup options is using the /etc/docker/daemon.json file.

#### Example for setting up access to an insecure registry

~~~~
cat /etc/docker/daemon.json

{
"insecure-registries":["server01.example.com:5000"]}
}
~~~~

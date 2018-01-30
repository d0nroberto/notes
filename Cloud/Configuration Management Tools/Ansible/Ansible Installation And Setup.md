# Ansible Installation & Setup

### What is Ansible?

Ansible is a configuration management / orchestration automation tool for managing a number of hosts.
It uses ssh and python to run a series of commands on a host.

The Ansible Control Server is the host that has the Ansible package installed.
The commands originate from this server.

The Remote Host is the host on which the commands are run.

The Control Server packages the commands into a python script. 
The script is then sent to /tmp on the remote host via ssh and run on the host.
The remote host then responds in JSON format with the output.

### Ansible Components

#### Inventory
The inventory is a list of hosts that ansible knows about and can run commands on. 
It is called the 'hosts' file for ansible.

e.g.

~~~~
# cat /opt/ansible/inventory
10.0.0.10
10.0.0.11
10.0.0.12
10.0.0.13
~~~~

#### Playbook
The playbook is the list of commands you want to run on the remote host.
The playbook is written in yaml format.

#### Ansible Config
This is a series of global config options for the ansible program.

#### Modules
These provide functionality to ansible. There are a set of core modules provided by default
and extra modules that allow ansible carry out more actions.

### Installation

~~~~
# yum -y install epel-release
# yum -y install ansible
~~~~

### Configuration

#### Simple configuration as follows:


1. Create inventory file with a list of host IPs
2. Create ssh key pair on Ansible Server
3. Copy the public key to .ssh/authorized_keys of all the Remote Hosts.

You should now be able to run various ansible commands e.g.

~~~~
# ansible all -i inventory -u ansible --private-key=/ansible/.ssh/id_rsa --become --ask-become-pass -m ping
# ansible all -i inventory -u ansible --private-key=/ansible/.ssh/id_rsa --become --ask-become-pass -m command -a "yum -y update"
~~~~


#### Command Options:

General Options:

-i INVENTORY, --inventory-file=INVENTORY
                        specify inventory host path
                        (default=/etc/ansible/hosts) or comma separated host list.

-m MODULE_NAME, --module-name=MODULE_NAME
                        module name to execute (default=command)

-a MODULE_ARGS, --args=MODULE_ARGS
                        module arguments

Connection Options: control as whom and how to connect to hosts

-u REMOTE_USER, --user=REMOTE_USER
                        connect as this user (default=None)

--private-key=PRIVATE_KEY_FILE, --key-file=PRIVATE_KEY_FILE
                        use this file to authenticate the connection

Privilege Escalation Options: control how and which user you become as on target hosts
    
-b, --become        run operations with become (does not imply password prompting)
    
-K, --ask-become-pass
                        ask for privilege escalation password                        


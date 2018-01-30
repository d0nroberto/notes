# Setting Up Vagrant on a Windows Host

#### Installation

First install virtualbox and vagrant onto your system
Once installed verify access to the 'vagrant' and 'vboxmanage' commands from the cli.


#### Setup

Open a terminal and create a folder. In the folder run the vagrant init command.

~~~~

mkdir testlab01

cd testlab01

vagrant init

~~~~

This creates a 'Vagrantfile' in the folder.


#### Configuring a Vagrantfile

Edit the Vagrantfile created in the folder tp specify machine details.
You can discover what boxes are available at https://app.vagrantup.com/boxes/search
Here are some samples:


To quickly spin up 3 ubuntu VMs:
~~~~

Vagrant.configure(2) do | config |

	config.vm.define "machine01" do |machine01|
		machine01.vm.box = "ubuntu/trusty64"
		machine01.vm.hostname = "machine01"
		machine01.vm.network "private_network", ip: "192.168.10.10"
	end

	config.vm.define "machine02" do |machine02|
		machine02.vm.box = "ubuntu/trusty64"
		machine02.vm.hostname = "machine02"
		machine02.vm.network "private_network", ip: "192.168.10.11"
		machine02.vm.network "forwarded_port", guest: 80, host:8080
	end

	config.vm.define "machine03" do |db|
		machine03.vm.box = "ubuntu/trusty64"
		machine03.vm.hostname = "machine03"
		machine03.vm.network "private_network", ip: "192.168.10.12"
	end

end

~~~~


To select a specific port to forward ssh on with multiple VMs:

~~~~

    machine01.vm.network :forwarded_port, guest: 22, host: 10222, id: "ssh"

~~~~


To customise VM specs:

~~~~

    machine01.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 512]
      v.customize ["modifyvm", :id, "--name", "machine01"]
    end

~~~~


#### Managing Machines

To check what images are local on a host

~~~~

vagrant box list

~~~~


To start / stop / destroy machines

~~~~

vagrant up

vagrant status

vagrant suspend machine01

vagrant resume machine01

vagrant port machine02

vagrant halt

vagrant status

vagrant destroy

vagrant status

~~~~


The vboxmanage command can also help

~~~~

vboxmanage list vms

vboxmanage list runningvms

vboxmanage showvminfo <UUID>

~~~~


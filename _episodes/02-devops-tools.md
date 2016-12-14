---
layout: episode
title: "Tools for automation"
teaching: 30
exercises: 10
questions:
  - "What does Vagrant do?"
  - "What are Ansible, Puppet and Chef?"
  - "What are the differences between Docker and LXC?"
objectives:
  - "You will learn basics of some DevOps tools"
keypoints:
  - "Vagrant, Ansible, Puppet, Chef, Docker, LXC"

---

# Introduction to some automation tools

# The lesson

- You will familiarize yourself with several automatization tools
  - Vagrant
  - Ansible
  - Puppet
  - Docker
  - LXC
- Some tools are covered in other lessons, for example Git, CMake and continuous integration

---

## Ansible, Chef and Puppet - configuration management tools

- These tools automatize configuration of the servers
- Highly scalable: easy to handle even hundreds or thousands of servers
- Combine with git and you also have versioning

### Puppet

- Open source configuration management tool
- Written in Ruby
- Relatively old
- Supports Unix-like and Microsoft Windows systems
- Vast variety of modules available

#### Architecture

- Client-server architecture: configuration mastered by puppet master
- Clients - puppet agents - connect to the puppet master to get configuration
- Can also run standalone
- Declare resources in manifest files
- No ordering -> you declare the final state of the machine

Write a manifest to install httpd:

```ruby
package { 'httpd':
    ensure => latest,
}

service { 'httpd':
    ensure => running,
    enable => true,
}
```

Apply it to the client with:

```shell
$ puppet agent -t
```


---

## Vagrant

- Tool to build and maintain virtual development environments
- Create a development environment -> distribute it to all developers
  - Fast to create a development environment
  - Everyone has identical environment

### Installation

- Install VirtualBox, [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Install Vagrant, from [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)
- You can use other VM providers as well, for example VMware

Example installation with RHEL, by not using the downloadable packets:

Note! You can get the newest Vagrant installable from [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

```shell
sudo cd /etc/yum.repos.d/
sudo wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
sudo yum search virtualbox
sudo yum install VirtualBox-5.1
cd
wget https://releases.hashicorp.com/vagrant/1.9.1/vagrant_1.9.1_x86_64.rpm
sudo yum install vagrant_1.9.1_x86_64.rpm
sudo /sbin/rcvboxdrv setup
```
Note: the versions for VirtualBox and Vagrant differ
Note: if you are using Fedora, change /rhel/ to /fedora/ in the link above

Installation on Ubuntu/Debian by not using the installation packages:

```shell
sudo apt-get install dkms
sudo apt-get install virtualbox
sudo apt-get install vagrant
```

To verify installation, run:

```shell
vagrant version
```

The vagrant box is built by the configuration in a file called _Vagrantfile_. It uses Ruby syntax, but it can be easily modified without any knowledge of the programming language. To test the example box first clone the file with:

```shell
git clone https://github.com/coderefinery/tools.git
```

To add the box, run:

```shell
vagrant box add centos/7
```
Choose the provider you want to use, in this example it is virtualbox

Change to the cloned directory (vagrant-basic) and run:

```shell
vagrant up
```

After the server is built, you can access it with command 

```shell
vagrant ssh
```

executed from the directory containing the Vagrantfile. You can also ssh the machine normally with ssh client, the vagrant user's default password is vagrant. If the script was executed correctly, you should also see httpd server responding via a forwarded port, from address [http://127.0.0.1:7888](http://127.0.0.1:7888).

Other common commands for vagrant are:

```shell
vagrant halt
vagrant destroy
vagrant up --provision
```

to stop, delete and reprovision a virtual machine, respectively. To create a Vagrantfile from scratch or to update an existing box, use commands:

```shell
vagrant init <box name>
vagrant box update
```


***

#### Additional information

To make the shared folder work withouth using rsync, you will need the VirtualBox Guest Additions installed. Guest additions provide several features for virtual machines, for example drivers. This example shows how to install these on VirtualBox 5.1.2. Normally you won't need this but in case you do, it is good to know about this package.

```
wget http://download.virtualbox.org/virtualbox/5.1.2/VBoxGuestAdditions_5.1.2.iso
sudo mkdir /media/VBoxGuestAdditions
sudo mount -o loop,ro VBoxGuestAdditions_5.1.2.iso /media/VBoxGuestAdditions
sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
rm VBoxGuestAdditions_5.1.2.iso
sudo umount /media/VBoxGuestAdditions
sudo rmdir /media/VBoxGuestAdditions
```

## Ansible

- Originally released in 2012
- New tool among configuration management automation tools
- Written in Python
- Has been rabidly gaining foothold
- Relatively easy and quick to start with
- Usually used as push model

## Chef

- Originally released in 2009
- Written in Ruby
- Master-client model
- Lot of modules available
- Not necessarily that simple to learn: need to know Ruby


---

# Containerisation

## Docker

- Docker provides containerisation in software level
- Available for most common operating systems, and also some clouds: AWS, Azure
- Provides an easy and fast way to spawn services
- With orchestration you can spawn new instances quickly and automatically to serve users in traffic peaks
- Public Docker images available in Docker hub [https://hub.docker.com/](https://hub.docker.com/]) but a word of warning: not all images can be trusted! There have been examples of contaminated images so investigate before using images blindly.
- Note that Docker containers should be disposable: the data must be saved elsewherei
- The image is built based on the Dockerfile

In this excercise you can use the Vagrant repository again, use the directory 'docker'. The Vagrantfile will again install Centos 7, but now with Docker and it will also start a docker image "jupyter/minimal-notebook". In Docker hub this image can be found behind url [https://hub.docker.com/r/jupyter/minimal-notebook/](https://hub.docker.com/r/jupyter/minimal-notebook/) and in GitHub under [https://github.com/jupyter/docker-stacks](https://github.com/jupyter/docker-stacks).

This Vagrantfile will first install Docker with:

```shell
yum install docker
```

After this it will start the Docker daemon, which is the persistent process managing the containers. The jupyter image will be pulled and started with command:

```shell
docker run -d -p 8888:8888 jupyter/minimal-notebook
```

The notebook should be accessible behind url http://127.0.0.1:8080 if everything went as expected (there is a redirect in Vagrantfile from port 8888 of guest machine to 8080 on host machine. You can also 'vagrant ssh' to the guest machine. You can now also access the container from shell. Find the container name (or id) with:

```shell
docker ps
```

Now you can execute shell in container with:

```shell
docker exec -it <id> bash
```

## LXC

- LXC is another container technology which provides system-level virtualization
- with LXC, you can run multiple isolated Linux systems on a single host -> while Docker's purpose is to deploy applications, LXC deploys virtual machines
- Still runs on the same kernel, so is not like having a virtual machine
- Isolation in userspace (namespaces) just like in FreeBSD jails



# Ansible Essential Training #

This repository holds my example files for
[LinkedIn's Ansible Essential Training](https://www.linkedin.com/learning/ansible-essential-training/). Since example files from LinkedIn are Copyright protected, I would recommend download it and uncompress it under [Exercise Files](Exercise Files) directory to keep reference on comments.

## Lab ##
To save time, I am shiping the laboratory out-of-the-box with Vagrant and VirtualBox (if you don't have it, install it first). Also if you're running a Linux box, I would advice install apt-proxy-ng, this will create a service on your host operating system that will cache every package installed by your guest machines (look at [Vagrantfile](Vagrantfile) to uncomment line that enable this apt repository). There is some tweaks in Vagrantfile needed to create the magic:

- SSH key authentication are done with vagrant insecure keys. [Vagrantfile](Vagrantfile) has *config.ssh.insert_key=false* which injects Vagrant's general public key into guess machines. Also>, Inventory files have instructions to use *vagrant* account (*ansible_ssh_user* in ini style, and *ansible_user* in yml style) and *~/.vagrant.d/insecure_private_key* as private key (*ansible_ssh_private_key_file* in ini style, and *ansible_private_key_file* in yml file)

- To avoid unknow host key behavior on first connect, it is configured *"-o StrictHostKeyChecking=no"* in inventory files ([inventory.ini](inventory.ini) and [inventory.yml](inventory.yml))

To boot up this laboratory the following command should be executed from same folder where reside files:

```shell
  vagrant up
```

Just in case you machines are not provisioned, an extra step could be executed: 

```shell
  vagrant provision
```

A quick check could be done to be sure about ansible will run on virtual machines:

```shell
  ansible -m ping -i inventory.yml all
```

This test all ansible targets on an inventory file are reachable. The same is done in [Exercise Files/ad-hoc.sh] except this script only test web01 node.

### 1-Setup ###
Files located at [Exercise Files/1-Setup](Exercise Files/1-Setup) are used to setup the machines. There is a main task to allow run next examples: installation of packet-python and netaddr, this is done through pip module in [Exercise Files/1-Setup/local.yml](Exercise Files/1-Setup/local.yml).

The laboratory shipped on this repository is using Debian official vagrant boxes which doesn't include pip installed by default, I fixed this in my [01-Setup.yml](01-Setup.yml), adding a previous task in which python-pip is installed using apt module.

To execute this exercise over all nodes execute

```shell
  ansible-playbook -i inventory.ini 01-Setup.yml
```

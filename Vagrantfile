# -*- mode: ruby -*-
# vi: set ft=ruby :

$repositories = <<-SCRIPT
tee <<SCRIPT>/etc/apt/sources.list
deb http://192.168.44.1:3142/deb.debian.org/debian stretch main contrib non-free
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "debian/buster64"
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # this will help on allow ssh connection out of the box
  # with vagrant insecure private key
  config.ssh.insert_key=false

  config.vm.define "web1" do |web1|
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: "192.168.44.11"
  end

  config.vm.define "web2" do |web2|
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "192.168.44.12"
  end

  config.vm.define "database1" do |database1|
    database1.vm.hostname = "database1"
    database1.vm.network "private_network", ip: "192.168.44.21"
  end

  config.vm.define "database2" do |database2|
    database2.vm.hostname = "database2"
    database2.vm.network "private_network", ip: "192.168.44.22"
  end

  # uncomment following block just if apt-cacher-ng is installed in host machine
  # config.vm.provision "shell", inline: $repositories

  config.vm.provision "shell",
    inline: "sudo apt update && apt install python-apt -y -o Dpkg::Options::=--force-confdef"
end

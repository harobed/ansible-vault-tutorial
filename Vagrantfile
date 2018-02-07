# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define :server do |server|
    server.vm.box = "ubuntu/xenial64"
    server.vm.synced_folder '.', '/vagrant', disabled: true
    server.vm.network "private_network", type: "dhcp"
    server.vm.provision "shell", inline: "sudo apt-get update -y; sudo apt-get install -y python"
  end
end

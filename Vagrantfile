# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"

  config.vm.provider "docker" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.define "docker" do |srv|
    srv.vm.network "private_network", ip: "192.168.56.10"
    srv.vm.hostname = "ansible"
#    srv.vm.provision "shell", path: "script.sh"
  end

end

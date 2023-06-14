# -*- mode: ruby -*-
# vi: set ft=ruby :
NUM_WORKER_NODES=2 
IP_NW="10.0.0."
IP_START=10
DASHBORD_VERSION="2.7.0"
Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update -y
    echo "$IP_NW$((IP_START)) master-node" >> /etc/hosts
    echo "$IP_NW$((IP_START+1)) worker-node1" >> /etc/hosts
    echo "$IP_NW$((IP_START+2)) worker-node2" >> /etc/hosts
  SHELL

  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_check_update = true

  # Creating the master node
  config.vm.define "master" do |master|
    # master.vm.box = "bento/ubuntu-18.04"
    master.vm.hostname = "master-node"
    master.vm.network "private_network", ip: IP_NW + "#{IP_START}"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master"
      vb.memory = 4096
      vb.cpus = 2
    end
    master.vm.provision "shell", path: "scripts/common.sh"
    master.vm.provision "shell", path: "scripts/master.sh"
  end

  # Creating the nodes
  (1..NUM_WORKER_NODES).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.hostname = "worker-node#{i}"
      node.vm.network "private_network", ip: IP_NW + "#{IP_START + i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "node#{i}"
        vb.memory = 2048
        vb.cpus = 1
      end
      node.vm.provision "shell", path: "scripts/common.sh"
      node.vm.provision "shell", path: "scripts/node.sh"

      # Only install the dashboard after provisioning the last worker (and when enabled).
      if i == NUM_WORKER_NODES and DASHBORD_VERSION and DASHBORD_VERSION != ""
        node.vm.provision "shell", path: "scripts/dashboard.sh"
      end
    end
  end
end
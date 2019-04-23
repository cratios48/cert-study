# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.provider "vitualbox" do |v|
    v.cpus = 1
    v.memory = 1024
  end
  config.vm.provision "shell", inline: "
    sudo timedatectl set-timezone Asia/Seoul
    sudo yum -y install vim
    sudo echo -e '10.10.10.10 server\n10.10.10.20 client' >> /etc/hosts
    "
  # Not sync vdi file
  # If want file provisioning, change vdi location
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "server" do |s|
    s.vm.hostname = "server"
    s.vm.network "private_network", ip: "10.10.10.10"
    
    serverDisk = "./serverExtraDisk.vdi"

    s.vm.provider "virtualbox" do |v|
      unless File.exist?(serverDisk)
        v.customize ['createmedium', 
                      '--filename', serverDisk, 
                      '--variant', 'Standard', 
                      '--size', 20 * 1024]
      end
      v.customize ['storageattach', 
                    :id, 
                    '--storagectl', 'IDE', 
                    '--port', 1, 
                    '--device', 1, 
                    '--type', 'hdd', 
                    '--medium', serverDisk]
    end
  end

  config.vm.define "client" do |c|
    c.vm.hostname = "client"
    c.vm.network "private_network", ip: "10.10.10.20"

    clientDisk = "./clientExtraDisk.vdi"

    c.vm.provider "virtualbox" do |v|
      unless File.exist?(clientDisk)
        v.customize ['createmedium', 
                      '--filename', clientDisk, 
                      '--variant', 'Standard', 
                      '--size', 20 * 1024]
      end
      v.customize ['storageattach', 
                    :id, 
                    '--storagectl', 'IDE', 
                    '--port', 1, 
                    '--device', 1,
                    '--type', 'hdd', 
                    '--medium', clientDisk]
    end
  end
end

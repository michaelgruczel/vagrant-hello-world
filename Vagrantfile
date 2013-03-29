# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu12041-CP-Vortrag"
  config.vm.box_url = "http://dl.dropbox.com/u/4031118/Vagrant/ubuntu-12.04.1-server-i686-virtual.box"
  config.vm.network :forwarded_port, guest: 8080, host: 4567
  config.vm.network :forwarded_port, guest: 80, host: 4568
  config.vm.synced_folder "./workspaces", "/vagrant_data"
  config.vm.provider :virtualbox do |vb|
    vb.gui = false
  end

  config.vm.provision :shell, :inline => "apt-get update"
  # Otherwise java install will fail with missing dependencies  

  config.vm.provision :chef_solo do |chef|
     chef.cookbooks_path = "cookbooks"
     chef.add_recipe "java"
  end

end

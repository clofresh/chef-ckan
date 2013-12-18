# -*- mode: ruby -*-
# vi: set ft=ruby :

root = File.expand_path("..", __FILE__)
chef_json = File.join(root, "solo.json")

Vagrant.configure("2") do |config|
  config.vm.box = "opscode-ubuntu-12.04"
  config.vm.box_url = "https://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-12.04_chef-provisionerless.box"

  # Must have vagrant-omnibus plugin: vagrant plugin install vagrant-omnibus
  config.omnibus.chef_version = :latest

  # Must have vagrant-berkshelf plugin: vagrant plugin install vagrant-berkshelf
  config.berkshelf.enabled = true

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

  config.vm.network "forwarded_port", guest: 8983, host: 8983
  config.vm.network "forwarded_port", guest: 5000, host: 5000

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.json = JSON.parse(File.read(chef_json))
    chef.json["user"] = "vagrant"
    chef.json["run_list"].each do |recipe|
      chef.add_recipe recipe
    end
  end

end

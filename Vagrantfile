# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "lucid64"
  config.vm.box_url = "http://files.vagrantup.com/lucid64.box"

  config.omnibus.chef_version = "11.6.0"
  config.cache.auto_detect = true

  # Set defaults until issue resolved:
  # https://github.com/mitchellh/vagrant/issues/1113
  chef_default = proc do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.data_bags_path = "data_bags"
    chef.add_recipe "apt"
    chef.add_recipe "vim"
  end

  config.vm.define :sensu_master do |sensu|
    sensu.vm.hostname = "sensu.local"
    sensu.vm.network :forwarded_port, guest: 8080, host: 8080
    sensu.vm.provision :chef_solo do |chef|
      chef_default.call(chef)
      chef.add_recipe "monitor::master"

      chef.json = {
        :sensu => {
          :use_ssl => false,
        },
      }
    end
  end

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "3000"]
  end

end

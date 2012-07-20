# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.box = "ubuntu-12.04-64"
  config.vm.box_url = "http://people.virginia.edu/~err8n/ubuntu-12.04-64.box"
  config.vm.network :hostonly, "33.33.33.25"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    # chef.roles_path     = "roles"
    # chef.data_bags_path = "data_bags"

    chef.add_recipe "voyant"
    chef.add_recipe "php"
    chef.add_recipe "apache2"
  
    chef.json = { :mysql_password => "foo" }
  end
end

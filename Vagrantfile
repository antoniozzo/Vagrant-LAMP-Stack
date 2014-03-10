# -*- mode: ruby -*-
# vi: set ft=ruby :



#### General project settings ####

# Set ip address
IP_ADDRESS = "172.22.22.22"

# Set project name & ext, used in domain
PROJECT_NAME = "myproject"
PROJECT_EXT = "local"

# Set db conig
DB_NAME = "mydb"
DB_PWD = "root"



#### Vagrant configuration ####

Vagrant.configure("2") do |config|

	# Enable Berkshelf support
	config.berkshelf.enabled = true

	# Define VM box to use
	config.vm.box = "precise32"
	config.vm.box_url = "http://files.vagrantup.com/precise32.box"

	# Set share folder
	config.vm.synced_folder "./" , "/var/www/" + PROJECT_NAME + "/", :mount_options => ["dmode=777", "fmode=666"]

	# Use hostonly network with a static IP Address and enable
	# hostmanager so we can have a custom domain for the server
	# by modifying the host machines hosts file
	config.hostmanager.enabled = true
	config.hostmanager.manage_host = true
	config.vm.define PROJECT_NAME do |node|
		node.vm.hostname = PROJECT_NAME + "." + PROJECT_EXT
		node.vm.network :private_network, ip: IP_ADDRESS
		node.hostmanager.aliases = ["www." + PROJECT_NAME + "." + PROJECT_EXT]
	end
	config.vm.provision :hostmanager

	# Make sure that the newest version of Chef have been installed
	config.vm.provision :shell, :inline => "apt-get update -qq && apt-get install make ruby1.9.1-dev --no-upgrade --yes"
	config.vm.provision :shell, :inline => "gem install chef --version 11.6.0 --no-rdoc --no-ri --conservative"

	# Enable and configure chef solo
	config.vm.provision :chef_solo do |chef|
		chef.add_recipe "app::packages"
		chef.add_recipe "app::web_server"
		chef.add_recipe "app::vhost"
		chef.add_recipe "memcached"
		chef.add_recipe "app::db"
		chef.json = {
			:app => {
				:name										=> PROJECT_NAME,
				:db_name								=> DB_NAME,
				:server_name						=> PROJECT_NAME + "." + PROJECT_EXT,
				:server_aliases					=> ["www." + PROJECT_NAME + "." + PROJECT_EXT],
				:docroot								=> "/var/www/" + PROJECT_NAME + "/www",
				:packages								=> %w{ vim git screen curl },
				:php_packages						=> %w{ php5-mysqlnd php5-curl php5-mcrypt php5-memcached php5-gd }
			},
			:mysql => {
				:server_root_password		=> DB_PWD,
				:server_repl_password		=> DB_PWD,
				:server_debian_password	=> DB_PWD,
				:bind_address						=> IP_ADDRESS,
				:allow_remote_root			=> true
			}
		}
	end

end

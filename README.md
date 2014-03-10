# Vagrant LAMP stack

## Installation
* Install [VirtualBox](https://www.virtualbox.org)
* Install [Vagrant](http://vagrantup.com)
* Install Berkshelf:
	* `gem install berkshelf`
* Install Berkshelf for Vagrant
	* `vagrant plugin install vagrant-berkshelf`
* Install Hostmanager for Vagrant
	* `vagrant plugin install vagrant-hostmanager`

## Usage
* Edit Vagrantfile project options
* `cd project-name`
* `vagrant up`
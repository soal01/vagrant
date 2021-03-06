# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/focal64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "VagrantFile"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  #config.vm.synced_folder "salt", "/srv/salt/"
  
  config.vm.provision :salt do |salt|
		salt.masterless = false
		salt.run_highstate = false
		config.ssh.pty = false
		
	end
  config.vm.define "master" do |master|
		master.vm.synced_folder "salt-files", "/srv/salt/"
		# "/srv/salt"
		master.vm.network "private_network", ip: "192.168.50.10"

		master.vm.provision :salt do |saltmaster|
			saltmaster.install_master = true
			saltmaster.minion_config = "configs/master"
			#pem
			saltmaster.minion_key = "keys/master/master.pem"
			saltmaster.minion_pub = "keys/master/master.pub"
			saltmaster.master_key = "keys/master/master.pem"
			saltmaster.master_pub = "keys/master/master.pub"
			saltmaster.seed_master = {
				master: 'keys/master/master.pub',
				minion: 'keys/minion/minion.pub'
			}
			saltmaster.install_type = "stable"
			saltmaster.verbose = true
			saltmaster.colorize = true
			saltmaster.bootstrap_options = "-F -P -c /tmp"
		end
	end
  
  config.vm.define "minion" do |minion|
		minion.vm.synced_folder "salt-files", "/srv/salt"
		minion.vm.network "private_network", ip: "192.168.50.20"

		minion.vm.provision :salt do |saltminion|
			saltminion.minion_config = "configs/minion"
			saltminion.minion_key = "keys/minion/minion.pem"
			saltminion.minion_pub = "keys/minion/minion.pub"
			saltminion.install_type = "stable"
			saltminion.verbose = true
			saltminion.colorize = true
			saltminion.bootstrap_options = "-F -P -c /tmp"
		end
	end
  
end

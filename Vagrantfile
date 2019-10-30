# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.ssh.private_key_path = "C:/Users/roland.mutter/.ssh/id_rsa"
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.define :master do |config|
	  config.vm.box = "ubuntu/trusty64"
	  config.vm.synced_folder "./share", "/home/vagrant/share"
	  config.vm.host_name = "master"
	  config.vm.network "public_network"
	  config.vm.network "private_network", ip: "10.145.0.10"
	  config.vm.provider :virtualbox do |vb|
			vb.customize ["modifyvm", :id, "--memory", "1024"]
			vb.customize ["modifyvm", :id, "--cpus", "2"]
	  end
	  config.vm.provision "shell", inline: <<-SHELL
		  apt-get update && apt-get install software-properties-common python -y
		  apt-add-repository --yes ppa:ansible/ansible && apt-get update
		  apt-get install ansible -y
		  apt-get install ansible-lint -y
		  echo -e "\n---- copy ssh keys from sync folder ----"
		  rm /home/vagrant/.ssh/authorized_keys
		  cp /home/vagrant/share/id_rsa /home/vagrant/.ssh/id_rsa
		  cp /home/vagrant/share/id_rsa.pub /home/vagrant/.ssh/authorized_keys
		  #cp /home/vagrant/share/id_rsa2.pub /home/vagrant/.ssh/authorized_keys
		  rm /./etc/ansible/hosts
		  cp /home/vagrant/share/hosts.txt /./etc/ansible/hosts
		  echo -e "\n---- ssh permisions set ----"
		  chmod 755 /home/vagrant/.ssh
		  chmod 400 /home/vagrant/.ssh/id_rsa
		  chmod 400 /home/vagrant/.ssh/authorized_keys
		  chown -R vagrant:vagrant /home/vagrant/.ssh
		SHELL
  end
  
  config.vm.define :node1 do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.box_version = "20190918.0.0"
    config.vm.host_name = "node1"
	config.vm.network "public_network"
    config.vm.network "private_network", ip: "10.145.0.11"
    config.vm.synced_folder "./share", "/home/vagrant/share"
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
    config.vm.provision "shell", inline: <<-SHELL
      apt-get update && apt-get install software-properties-common python -y
      echo -e "\n---- copy ssh keys from sync folder ----"
	  rm /home/vagrant/.ssh/authorized_keys
	  cp /home/vagrant/share/id_rsa /home/vagrant/.ssh/id_rsa
	  cp /home/vagrant/share/id_rsa.pub /home/vagrant/.ssh/authorized_keys
	  #cp /home/vagrant/share/id_rsa2.pub /home/vagrant/.ssh/authorized_keys
	  echo -e "\n---- ssh permisions set ----"
	  chmod 755 /home/vagrant/.ssh
	  chmod 400 /home/vagrant/.ssh/id_rsa
	  chmod 400 /home/vagrant/.ssh/authorized_keys
	  chown -R vagrant:vagrant /home/vagrant/.ssh
      apt-get purge apache2 -y
    SHELL
  end
  
  config.vm.define :node2 do |config|
    config.vm.box = "geerlingguy/centos7"
    config.vm.host_name = "node2"
	config.vm.network "public_network"
    config.vm.network "private_network", ip: "10.145.0.12"
    config.vm.synced_folder "./share", "/home/vagrant/share"
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
    config.vm.provision "shell", inline: <<-SHELL
      yum update && yum install software-properties-common python -y
      echo -e "\n---- copy ssh keys from sync folder ----"
	  rm /home/vagrant/.ssh/authorized_keys
	  cp /home/vagrant/share/id_rsa /home/vagrant/.ssh/id_rsa
	  cp /home/vagrant/share/id_rsa.pub /home/vagrant/.ssh/authorized_keys
	  #cp /home/vagrant/share/id_rsa2.pub /home/vagrant/.ssh/authorized_keys
	  echo -e "\n---- ssh permisions set ----"
	  chmod 755 /home/vagrant/.ssh
	  chmod 400 /home/vagrant/.ssh/id_rsa
	  chmod 400 /home/vagrant/.ssh/authorized_keys
	  chown -R vagrant:vagrant /home/vagrant/.ssh
	  echo -e "\n---- Node 1 and Node 2 are ready, make sure you use ansible playbooks from master ----"
    SHELL
  end
  
  config.vm.define :node3 do |config|
	config.vm.box = "ubuntu/bionic64"
	config.vm.synced_folder "./share", "/home/vagrant/share"
	config.vm.host_name = "node3"
	config.vm.network "public_network"
	config.vm.network "private_network", ip: "10.145.0.13"
	config.vm.provider :virtualbox do |vb|
		vb.customize ["modifyvm", :id, "--memory", "1024"]
		vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
    config.vm.provision "shell", inline: <<-SHELL
	  apt-get update && apt-get install software-properties-common python -y
	  apt-add-repository --yes ppa:ansible/ansible && apt-get update
	  apt-get install ansible -y
	  apt-get install ansible-lint -y
	  echo -e "\n---- Install Jenkins ----"
	  wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
	  echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
	  apt-get update
	  apt-get install jenkins -y
	  apt-get install openjdk-8-jre -y
	  echo -e "\n---- copy ssh keys from sync folder ----"
	  rm /home/vagrant/.ssh/authorized_keys
	  cp /home/vagrant/share/id_rsa /home/vagrant/.ssh/id_rsa
	  cp /home/vagrant/share/id_rsa.pub /home/vagrant/.ssh/authorized_keys
	  #cp /home/vagrant/share/id_rsa2.pub /home/vagrant/.ssh/authorized_keys
	  rm /./etc/ansible/hosts
	  cp /home/vagrant/share/hosts.txt /./etc/ansible/hosts
	  echo -e "\n---- ssh permisions set ----"
	  chmod 755 /home/vagrant/.ssh
	  chmod 400 /home/vagrant/.ssh/id_rsa
	  chmod 400 /home/vagrant/.ssh/authorized_keys
	  chown -R vagrant:vagrant /home/vagrant/.ssh
	  rm /./etc/init.d/jenkins
	  cp /home/vagrant/share/jenkins.txt /./etc/init.d/jenkins
	  systemctl deamon-reload
	  service jenkins start
	SHELL
  end
  
  config.vm.define :node4 do |config|
	config.vm.box = "ubuntu/bionic64"
	config.vm.synced_folder "./share", "/home/vagrant/share"
	config.vm.host_name = "node4"
	config.vm.network "public_network"
	config.vm.network "private_network", ip: "10.145.0.14"
	config.vm.provider :virtualbox do |vb|
		vb.customize ["modifyvm", :id, "--memory", "1024"]
		vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
    config.vm.provision "shell", inline: <<-SHELL
	  apt-get update && apt-get install software-properties-common python -y
	  echo -e "\n---- copy ssh keys from sync folder ----"
	  rm /home/vagrant/.ssh/authorized_keys
	  cp /home/vagrant/share/id_rsa /home/vagrant/.ssh/id_rsa
	  cp /home/vagrant/share/id_rsa.pub /home/vagrant/.ssh/authorized_keys
	  echo -e "\n---- ssh permisions set ----"
	  chmod 755 /home/vagrant/.ssh
	  chmod 400 /home/vagrant/.ssh/id_rsa
	  chmod 400 /home/vagrant/.ssh/authorized_keys
	  chown -R vagrant:vagrant /home/vagrant/.ssh
	SHELL
  end
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

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
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.ssh.private_key_path = "./share/id_rsa"
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
	  echo -e "\n---- ssh permisions set ----"
	  chmod 755 /home/vagrant/.ssh
	  chmod 400 /home/vagrant/.ssh/id_rsa
	  chmod 400 /home/vagrant/.ssh/authorized_keys
	  chown -R vagrant:vagrant /home/vagrant/.ssh
      apt-get purge apache2 -y
	  apt-get install collectd collectd-utils -y
	  cp /home/vagrant/share/collectd.conf /etc/collectd/
	  service collectd stop
	  service collectd start
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
	  echo -e "\n---- ssh permisions set ----"
	  chmod 755 /home/vagrant/.ssh
	  chmod 400 /home/vagrant/.ssh/id_rsa
	  chmod 400 /home/vagrant/.ssh/authorized_keys
	  chown -R vagrant:vagrant /home/vagrant/.ssh
	  apt-get install collectd collectd-utils -y
	  cp /home/vagrant/share/collectd.conf /etc/collectd/
	  service collectd stop
	  service collectd start
	  echo -e "\n PLEASE CHANGE Hostname Node1 ACCORDINGLY WITH THE NAME OF THIS MACHINE IN /etc/collectd/collectd.conf"
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
  
  # Node 4 is not 100% functional without manually configuring it. I'll let those here for further references
  config.vm.define :node4 do |config|
	config.vm.box = "ubuntu/xenial64"
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
	  apt-get install graphite-web graphite-carbon -y
	  apt-get install postgresql libpg-dev python-psycopg2 -y
	  apt-get install apache2 libapache2-mod-wsgi -y
	  cp /home/vagrant/share/graphite-carbon.txt /etc/default/graphite-carbon
	  cp /home/vagrant/share/carbon.txt /etc/carbon/carbon.conf
	  a2dissite 000-default
	  a2ensite apache2-graphite
	  systemctl restart apache2
	  ufw allow 80
	  apt-get install -y gnupg2 curl
	  curl https://packages.grafana.com/gpg.key | apt-key add -
	  add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
	  apt-get update
	  apt-get install grafana -y
	  systemctl start grafana-server
	  systemctl enable grafana-server
	  ufw allow proto tcp from any to any port 3000
      echo -e "\n MAKE SURE TO RUN THE FOLLOWING COMMANDS TO CREATE A DB FOR GRAPHITE:
	  \n sudo -u postgres psql
	  \n CREATE USER graphite WITH PASSWORD 'roli';
	  \n CREATE DATABASE graphite WITH OWNER graphite;
	  \n sudo cp /home/vagrant/share/local_settings.py /etc/graphite/local_settings.py
	  \n sudo graphite-manage migrate auth
	  \n sudo graphite-manage syncdb
	  \n sudo systemctl start carbon-cache"
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

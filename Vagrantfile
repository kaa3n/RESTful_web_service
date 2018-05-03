$script = <<SCRIPT
sudo timedatectl set-timezone America/Chicago
# set up install of Jenkins
#Create jenkins user
sudo useradd -m -s /bin/bash jenkins
#Set password for the jenkins user
sudo chpasswd << 'END'
jenkins:jenkins
END
echo 'jenkins user and password created'
#Step 4-Add jenkins user to sudo group
sudo usermod -a -G sudo jenkins
#Install Jenkins
sudo wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update -y
sudo apt-get install -y jenkins
sudo apt-get install -y php
sudo apt-get install -y default-jre
sudo apt-get install -y default-jdk
sudo apt-get install -y maven
sudo wget http://localhost:8080/jnlpJars/jenkins-cli.jar
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 install-plugin maven-plugin
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 install-plugin parameterized-trigger
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 install-plugin build-pipeline-plugin
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 install-plugin clone-workspace-SCM
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 install-plugin job-import-plugin
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 install-plugin config-file-provider
#Restart jenkins
#sudo java -jar jenkins-cli.jar -s http://localhost:8080 safe-restart
sudo apt-get update -y
sudo systemctl start jenkins
sudo mkdir -p /home/vagrant/.m2
sudo ufw allow 8080
SCRIPT
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
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.provider "virtualbox" do |vm|
    vm.name = "jenkins_server"
    vm.memory = 4000
    vm.cpus = 1
  config.vm.network "private_network", ip: "192.168.56.65"
  config.vm.network :forwarded_port, guest: 8080, host: 4567
  config.vm.synced_folder "c:/RESTful_web_service", "/home/vagrant", type: "virtualbox"
  config.vm.synced_folder "c:/Users/User1/.m2/", "/var/lib/jenkins/.m2", type: "virtualbox"
  config.vm.provision "shell", inline: $script
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

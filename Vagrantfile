# -*- mode: ruby -*-
# vi: set ft=ruby :
#TODO
#puppet libraryan
#puppet vagrant installer
Vagrant.configure(2) do |config|

  config.ssh.pty = true

  hosts = {
    'el1' => { 'ip' => '10.42.0.101', 'cpus' => 1, 'memory' => 1024 },
    'el2' => { 'ip' => '10.42.0.102', 'cpus' => 1, 'memory' => 1024 },
    'el3' => { 'ip' => '10.42.0.103', 'cpus' => 1, 'memory' => 1024 },
	'ls1' => { 'ip' => '10.42.0.104', 'cpus' => 1, 'memory' => 1024 }
  }

  hosts.each do |host, params|
    config.vm.define host, autostart: true do |host_config|
      host_config.vm.box = "nrel/CentOS-6.5-x86_64"
      host_config.vm.hostname = "#{host}"
      host_config.vm.network :private_network, ip: params['ip']

      host_config.vm.provider :virtualbox do |virtualbox|
        virtualbox.memory = params['memory']
        virtualbox.cpus = params['cpus']
      end
	  
      host_config.vm.provision :shell, inline: <<-SHELL
        sudo yum -y vim-enhanced
        sudo chkconfig iptables off
        sudo service iptables stop
        sudo rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-el-6.noarch.rpm
        sudo yum -y install puppet
		#Work around to java  version required for elastic. 
		#wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u65-b17/jdk-8u65-linux-x64.rpm"
		#sudo yum localinstall jdk-8u65-linux-x64.rpm -y
		#sudo yum localinstall /vagrant/jdk-8u65-linux-x64.rpm -y
		#sudo rpm --import http://packages.elastic.co/GPG-KEY-elasticsearch
      SHELL

      host_config.vm.provision :puppet do |puppet|
        puppet.module_path = "modules"
        puppet.manifests_path = "manifests"
        puppet.manifest_file = "default.pp"
      end
    end
  end
end

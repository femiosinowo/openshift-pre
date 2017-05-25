# -*- mode: ruby -*-
# # vi: set ft=ruby :
 
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
 
# Require YAML module
require 'yaml'
 
# Read YAML file with box details
servers = YAML.load_file('servers.yaml')
 

		# Create boxes
		Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|


		  servers.each do |servers|
			config.vm.define servers["hostname"] do |srv|
			  srv.vm.box = servers["box"]
			  srv.vm.network "private_network", ip: servers["ip"]

			  srv.vm.hostname =  servers["hostname"] + ".paosin.local"
			  srv.vm.provider :virtualbox do |vb|
				vb.name = servers["hostname"]
				vb.memory = servers["ram"]		
			  end
			  
			  machine = servers["hostname"] + ".paosin.local"
			  
			  if  servers["hostname"] == "os-mgmt"
			     config.vm.synced_folder "site", "/etc/ansible/",
				 	:owner => "root",
				 	:group => "root",
				 	:mount_options  => ['dmode=755,fmode=755']
				#config.vm.provision :shell, :path => "bootstrap-mgmt.sh", :args => serverrole
			  end
			  
			  serverrole =  servers["server_role"]
			  
			end
		  end
   
				

			  config.vm.provision "shell" , keep_color:true do |s|
				ssh_pub_key = File.readlines("id_rsa.pub").first.strip
				s.inline = <<-SHELL
				  echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
				  echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
				SHELL
			  end
  
				
  
	end
	

#
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # create some app servers
  (1..2).each do |i|
    config.vm.define "web#{i}" do |node|
        node.vm.box = "trusty64"
        node.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
        node.vm.hostname = "web#{i}"
        node.vm.boot_timeout="600"
        node.vm.network :private_network, ip: "10.0.15.2#{i}"
		#node.vm.network "forwarded_port", guest: 80, host: "808#{i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
          #vb.gui="true"
        end
        node.vm.provision :ansible do |ansible|
			ansible.groups = {
			'appserver' => ['web[1:2]'],
			'loadbalancer' => ['lb'] 
			} 
            ansible.playbook = "prov/webserver.yml"
        end 
    end
  end
 
 
  # create load balancer
  config.vm.define :lb do |lb_config|
      lb_config.vm.box = "trusty64"
      lb_config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
      lb_config.vm.hostname = "lb"
      lb_config.vm.boot_timeout="600"
      lb_config.vm.network :private_network, ip: "10.0.15.11"
      #lb_config.vm.network "forwarded_port", guest: 80, host: 8080
      lb_config.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
        #vb.gui="true"
      end
      lb_config.vm.provision :ansible do |ansible|
         ansible.groups = {
         'loadbalancer' => ['lb'],
		 'appserver' => ['web[1:2]']
		} 
        ansible.playbook = "prov/loadbalancer.yml"
      end
  end

end


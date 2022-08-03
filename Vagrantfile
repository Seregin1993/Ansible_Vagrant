# -*- mode: ruby -*-
# vi: set ft=ruby :
nodes = [
  { :hostname => 'sowa1', :ip => '192.168.56.20', :guestport => 80, :hostport => 8080 },
  { :hostname => 'sowa2', :ip => '192.168.56.21', :guestport => 80, :hostport => 8081 },
  { :hostname => 'sowa3', :ip => '192.168.56.22', :guestport => 80, :hostport => 8082 }
]
 
Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = 'centos/7'
      nodeconfig.vm.hostname = node[:hostname] + '.test.local'
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.network "forwarded_port", guest: node[:guestport], host: node[:hostport]
      
	nodeconfig.vm.provider :virtualbox do |v|
        v.memory = "4096"
        v.cpus = 2
        v.name = node[:hostname] 
      end
    end
   end
   
  config.vm.define "sowa1" do |node|
  node.vm.provision "shell", path: "provision/update-nss.sh"
  node.vm.provision  "ansible", playbook: "provision/ansible-freeipa/install-server.yml", inventory_path:"provision/ansible-freeipa/hosts_ipa", limit: "all"
  end
  
  config.vm.define "sowa2" do |node|
  node.vm.provision  "ansible", playbook: "provision/freeipa_client.yml", inventory_path:"provision/ansible-freeipa/hosts_ipa", limit: "all"
  node.vm.provision  "ansible", playbook: "provision/foreman/install-foreman.yml", inventory_path:"provision/foreman/inventory", limit: "all"
  end
  
  config.vm.define "sowa3" do |node|
  node.vm.provision "shell", path: "provision/install_pytnon-docker.sh"
  node.vm.provision  "ansible", playbook: "provision/install-docker.yml", inventory_path:"provision/inventory_docker", limit: "all"
  node.vm.provision  "ansible", playbook: "provision/compose.yml", inventory_path:"provision/inventory_docker", limit: "all"
  node.vm.provision  "ansible", playbook: "provision/install_zabbix.yml", inventory_path:"provision/inventory", limit: "all"
  end
  
 end
 



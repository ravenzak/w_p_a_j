# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--memory", 256]
  end

  config.vm.define :haproxy, primary: true do |haproxy_config|

    haproxy_config.vm.hostname = 'haproxy'
    haproxy_config.vm.network :forwarded_port, guest: 8080, host: 8080
    haproxy_config.vm.network :forwarded_port, guest: 80, host: 8081

    haproxy_config.vm.network :private_network, ip: "192.168.56.103"
    haproxy_config.vm.provision :shell, :path => "script/haproxy-setup.sh"

  end
  config.vm.define :web1 do |web1_config|

    web1_config.vm.hostname = 'web1'
    web1_config.vm.network :private_network, ip: "192.168.56.101"
    web1_config.vm.provision :shell, :path => "script/web-setup.sh"


  end
  config.vm.define :web2 do |web2_config|

    web2_config.vm.hostname = 'web2'
    web2_config.vm.network :private_network, ip: "192.168.56.102"
    web2_config.vm.provision :shell, :path => "script/web-setup.sh"
  end

config.vm.define :jenkins do |jenkins_config|

jenkins_config.vm.box = "centos/7"
jenkins_config.vm.hostname = "jenkins"
#jenkins_config.vm.forvard_port 8080

jenkins_config.vm.network "private_network", ip: "192.168.56.104"
jenkins_config.vm.synced_folder "." , "/chi", disble: true
jenkins_config.ssh.forward_agent = true
jenkins_config.ssh.insert_key = false
jenkins_config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key"]
end

config.vm.provision "ansible" do |ansible_config|

ansible_config.playbook = "provisioning/playbook.yml"
ansible_config.inventory_path = "provisioning/inventory"
ansible_config.sudo = true
end


end

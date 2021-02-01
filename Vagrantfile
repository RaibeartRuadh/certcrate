# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
    :server => {
	:box_name => "centos/7",
	:net => [
	    {ip: '192.168.100.100', adapter: 2},
	]
    }
  
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
	box.vm.box = boxconfig[:box_name]
	box.vm.host_name = boxname.to_s
	if boxconfig.key?(:forwarded_port)
	    boxconfig[:forwarded_port].each do |fp|
		box.vm.network "forwarded_port", fp
	    end
	end
	boxconfig[:net].each do |ipconf|
	  box.vm.network "private_network", ipconf
	end
	if boxconfig.key?(:public)
	  box.vm.network "public_network", boxconfig[:public]
	end
	box.vm.provider :virtualbox do |vb|
	    vb.customize ["modifyvm", :id, "--memory", "512"]
	end
	box.vm.provision "ansible" do |ansible|
	    #ansible.compatibility_mode = "2.0"
	    ansible.playbook = "playbook/main.yml"
            ansible.verbose = "v"
	    #ansible.become = "true"
	end
    end
  end
end





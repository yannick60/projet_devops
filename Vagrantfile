# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# Toutes les VMs sont definies dans ce fichier
hosts = YAML.load_file('hosts.yml')

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    hosts.each do |host|
        config.vm.define host['name'] do |node|
            # - HOST SETTINGS -
            node.vm.hostname = host['name']
            node.vm.network :private_network, ip: host['ip']
            node.vm.box = host['os'] 

            # - PROVISION -
            if host['provisioner'] == 'shell'
                node.vm.provision "shell",privileged: true, path: host['playbook']
            elsif host['provisioner'] == 'ansible'
                node.vm.provision "ansible" do |ansible|
                    ansible.playbook = host['playbook']
                    ansible.verbose = "True"
                end
            end

            # - PROVIDER -
            node.vm.provider :virtualbox do |vb|
                vb.gui = false
                vb.name = host['name']
                vb.customize ["modifyvm", :id, "--memory", host['mem'], "--cpus", host['cpus'], "--hwvirtex", "on","--groups", "/usine-logicielle","--natdnshostresolver1", "on", "--cableconnected1", "on"]
            end      
        end
    end
end
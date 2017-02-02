# -*- mode: ruby -*-
# vi: set ft=ruby :
#

hosts = [
  { id: '1', name: 'lemp', hostname: 'app', ip: '192.168.33.108', guest_port: '80', host_port: '8080'},
]

Vagrant.configure("2") do |config|
  hosts.each do |host|
    config.vm.define host[:name] do |c|
      c.ssh.insert_key = false
      c.vm.box = "ubuntu/trusty64"
      c.vm.synced_folder "app", "/var/www/app"
      c.vm.hostname = host[:hostname]
      c.vm.network "forwarded_port", guest: '80', host: '8080'
      c.vm.network "forwarded_port", guest: '3306', host: '33306'
      c.vm.network :private_network, ip: host[:ip], netmask: "255.255.255.0"
      c.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end
      c.vm.provision "ansible_local" do |ansible|
        ansible.playbook       = 'ansible/site.yml'
        ansible.verbose        = true
        ansible.install        = true
        ansible.limit          = "all" # or only "nodes" group, etc.
        ansible.inventory_path = "ansible/inventory/inventory.ini"
      end
    end
  end
end

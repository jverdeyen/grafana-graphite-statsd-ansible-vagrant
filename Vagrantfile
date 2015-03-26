# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "http://files.vagrantup.com/trusty64.box"

  private_ip = "10.0.0.20"
  cpu = ENV['VAGRANT_CPU'] || 2
  ram = ENV['VAGRANT_MEMORY'] || 1024

  config.vm.network :private_network, ip: private_ip

  config.vm.provider :virtualbox do |vm, override|
    vm.customize ["modifyvm", :id, "--memory", ram]
    vm.customize ["modifyvm", :id, "--cpus", cpu]
    #vm.gui = true # for debug
  end

  if Vagrant.has_plugin?("vagrant-hostsupdater")
    # https://github.com/cogitatio/vagrant-hostsupdater
    config.hostsupdater.aliases = ['stats.webfolks.be']
  end

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = 'v'
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "vagrant_hosts"
    ansible.limit          = "all"
    ansible.extra_vars     = {
      target: private_ip,
      user: 'vagrant'
    }
  end
end

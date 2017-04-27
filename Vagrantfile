# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.hostname = "localhost"
  config.vm.box = "ubuntu/trusty64"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision :shell, :inline => "sudo apt update && sudo DEBIAN_FRONTEND=\"noninteractive\" apt install -y python-minimal aptitude"

  config.vm.provision :ansible_local do |ansible|
    ansible.playbook = "/vagrant/setup_vagrant.yml"
    ansible.limit    = ""
  end

  config.vm.provision :ansible_local do |ansible|
    ansible.playbook   = "/vagrant/setup.yml"
    ansible.limit      = ""
    ansible.extra_vars = {
      builddir: "/vagrant/"
    }
  end

end

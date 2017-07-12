# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "asseth"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "4096"
    vb.name = "asseth"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--vram", "128"]
  end

  config.vm.network "forwarded_port", guest: 30303, host: 30303
  config.vm.network "forwarded_port", guest: 8545, host: 8545

  config.vm.synced_folder ".", "/vagrant", type: "rsync"

  config.vm.define "assethbox" do |assethbox|
  end


  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-add-repository ppa:ansible/ansible
    sudo apt-get update
    sudo apt-get install -y software-properties-common cowsay sshpass language-pack-fr python-pip ansible
    sudo pip install markupsafe
    su - vagrant -c 'ssh-keygen -t rsa -f "$HOME/.ssh/id_rsa" -q -N ""'
    su - vagrant -c 'cat /home/vagrant/.ssh/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys'
    su - vagrant -c 'echo StrictHostKeyChecking no >> /home/vagrant/.ssh/config'
    sudo service ssh restart
    su - vagrant -c 'sshpass -p vagrant ssh-copy-id vagrant@asseth'
    su - vagrant -c 'ansible-playbook /vagrant/provisioning/bootstrap.yml -i /vagrant/provisioning/hosts -vvvv'
    sudo reboot
  SHELL
end

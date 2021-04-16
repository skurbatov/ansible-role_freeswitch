# -*- mode: ruby -*-
# vi: set ft=ruby :
goss_version="v0.3.6"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    # vb.memory = "2048"
  end

  config.vm.network "forwarded_port", guest: 5060, host: 8060
  config.vm.network "forwarded_port", guest: 8021, host: 8021
  config.vm.network "forwarded_port", guest: 5080, host: 8080
  config.vm.network "forwarded_port", guest: 5061, host: 8061
  config.vm.network "forwarded_port", guest: 5081, host: 5081
  config.vm.network "forwarded_port", guest: 7443, host: 7443
  config.vm.network "forwarded_port", guest: 5070, host: 8070
  config.vm.network "forwarded_port", guest: 8021, host: 8021
  for i in 64535..65535
    config.vm.network :forwarded_port, guest: i, host: i
  end

  config.vm.provision "shell", inline: "apt-get install -y python3"

  config.vm.provision "shell", inline: <<-SHELL
    curl -s -L https://github.com/aelsabbahy/goss/releases/download/#{goss_version}/goss-linux-amd64 -o /usr/local/bin/goss
    chmod a+x /usr/local/bin/goss
  SHELL

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.galaxy_roles_path = "./tests/roles/"
    ansible.playbook = "tests/vagrant.yml"
    ansible.raw_arguments = ["--diff"]
  end

  config.vm.provision "file", source: "./tests/goss.yml", destination: "/tmp/goss.yml"
  config.vm.provision "shell", inline: "goss -g /tmp/goss.yml validate"
end

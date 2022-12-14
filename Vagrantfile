# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

# Check for required plugin(s)
['vagrant-hostmanager'].each do |plugin|
  unless Vagrant.has_plugin?(plugin)
    raise "#{plugin} plugin not found. Please install it via 'vagrant plugin install #{plugin}'"
  end
end


Vagrant.configure("2") do |config|

if Vagrant.has_plugin?('vagrant-hostmanager') then
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true
  config.hostmanager.manage_host = false
end

if Vagrant.has_plugin?('vagrant-vbguest') then
  config.vbguest.auto_update = false
end


config.vm.provider "virtualbox" do |h, override|
  h.gui = false
  h.memory = 2048
  h.cpus = 2

end

config.vm.synced_folder '.', '/vagrant', disabled: true
config.vm.box_check_update = false
config.ssh.forward_agent = true
config.ssh.insert_key = false
#config.ssh.extra_args = ["-O", "HostkeyAlgorithms", "+ssh-rsa", "-O", "PubkeyAcceptedAlgorithms", "+ssh-rsa"]
config.vm.define "ansible", primary: true do |ansible|
  ansible.vm.box = "generic/ubuntu2204"
  ansible.vm.hostname = "ansible"
  ansible.vm.network "private_network", ip: "192.168.56.100"

  ansible.vm.provision "file", source: "#{Dir.home}/.vagrant.d/insecure_private_key", destination: ".ssh/id_rsa"
  ansible.vm.provision "shell", privileged: false, inline: <<-SHELL
    chmod 600 ~vagrant/.ssh/id_rsa
  SHELL

  ansible.vm.provision "shell", inline: <<-SHELL
    apt update
    apt install -y python3-pip
  SHELL

  ansible.vm.provision "shell", privileged: false, inline: <<-SHELL
    python3 -m pip install --user ansible
  SHELL
  
 end

config.vm.define "ubuntulab" do |ubuntu|
  ubuntu.vm.box = "generic/ubuntu2204"
  ubuntu.vm.hostname = "ubuntulab"
  ubuntu.vm.network "private_network", ip: "192.168.56.20"
  ubuntu.vm.provision "shell", inline: <<-SHELL
    apt update
    apt upgrade
  SHELL
end

end

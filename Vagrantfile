# -*- mode: ruby -*-
# vim: set ft=ruby :
GLOBAL_VARS ={}
MACHINES = {
  :inet1Router => {
    :box_name => "almalinux/9",
    :net => [
      {adapter: 2, auto_config: false, virtualbox__intnet: "router-net"},
      {adapter: 3, auto_config: false, virtualbox__intnet: "router-net"},
      {ip: '192.168.56.10', adapter: 8},
    ],
    :vars => {bond_ip: '192.168.255.1'}
  },
  :inet2Router => {
    :box_name => "almalinux/9",
    :net => [
      {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
      {ip: '192.168.56.32', adapter: 8},
    ],
    :vars => {vlan_id: 2, vlan_ip: '10.10.10.1'}
  },
  
  :centralRouter => {
    :box_name => "almalinux/9",
    :net => [
      {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
      {ip: '192.168.56.32', adapter: 8},
    ],
    :vars => {vlan_id: 2, vlan_ip: '10.10.10.1'}
  },
  
  :centralServer => {
    :box_name => "almalinux/9",
    :net => [
      {adapter: 2, auto_config: false, virtualbox__intnet: "testLAN"},
      {ip: '192.168.56.32', adapter: 8},
    ],
    :vars => {vlan_id: 2, vlan_ip: '10.10.10.1'}
  },
}

Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  config.vm.provider :virtualbox
  config.vm.provider "virtualbox" do |vb|
    vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxname.to_s
      boxconfig[:net].each do |ipconf|
        box.vm.network "private_network", **ipconf
      end
      box.vm.provision "ansible" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "ansible/playbook.yml"
        ansible.extra_vars = GLOBAL_VARS.merge(boxconfig[:vars]).merge({host_name: boxname.to_s})
        ansible.host_key_checking = "false"
        ansible.become = "true"
        ansible.limit = "all"
      end
    end
  end
end

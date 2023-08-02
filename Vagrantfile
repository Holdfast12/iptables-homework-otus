# -*- mode: ruby -*-
# vim: set ft=ruby :

GLOBAL_VARS ={local_repo: '10.0.0.2'}

MACHINES = {

  :inetRouter => {
    :box_name => "almalinux/9",
    :net => [
      {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "central-inet"},
    ],
    :vars => {}
  },

  :inetRouter2 => {
    :box_name => "almalinux/9",
    :net => [
      {ip: '192.168.255.5', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "central-inet2"},
    ],
    :vars => {}
  },
  
  :centralRouter => {
    :box_name => "almalinux/9",
    :net => [
      {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "central-inet"},
      {ip: '192.168.255.6', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "central-inet2"},
      {ip: '192.168.0.1', adapter: 4, netmask: "255.255.255.252", virtualbox__intnet: "central-server"},
    ],
    :vars => {}
  },
  
  :centralServer => {
    :box_name => "almalinux/9",
    :net => [
      {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "central-server"},
    ],
    :vars => {}
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
      if boxname.to_s == "inetRouter2"
        box.vm.network "forwarded_port", guest: 8080, guest_ip: "192.168.255.5", host: 8888, host_ip: "127.0.0.1",  protocol: "tcp"
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

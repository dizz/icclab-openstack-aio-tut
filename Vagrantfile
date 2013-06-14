# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.hostname = "aio.cloudcomplab.dev"

  config.vm.box = "raring-server-cloudimg-vagrant-amd64-disk1"
  config.vm.box_url = "http://cloud-images.ubuntu.com/raring/current/raring-server-cloudimg-vagrant-amd64-disk1.box"

  # eth1: internal management
  config.vm.network :private_network, ip: "10.10.100.51", netmask: "255.255.255.0", nic_type: '82545EM'
  # eth2 public {in,e}gress
  config.vm.network :private_network, ip: "192.168.100.51", netmask: "255.255.255.0", nic_type: '82545EM'
  
  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    # vb.gui = true
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    #eth2 - needs to be set in promisc mode from within the hypervisor too
    vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
  end
end

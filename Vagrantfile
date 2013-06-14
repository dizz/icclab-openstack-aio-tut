# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.hostname = "aio.cloudcomplab.dev"

  config.vm.box = "raring-server-cloudimg-vagrant-amd64-disk1"
  config.vm.box_url = "http://cloud-images.ubuntu.com/raring/current/raring-server-cloudimg-vagrant-amd64-disk1.box"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # [--nictype<1-N> Am79C970A|Am79C973|82540EM|82543GC|82545EM|virtio]
  # [--nicpromisc<1-N> deny|allow-vms|allow-all]

  # eth1
  config.vm.network :private_network, ip: "10.10.100.51", netmask: "255.255.255.0", nic_type: '82545EM'
  # eth2 pub mgt
  config.vm.network :private_network, ip: "192.168.100.51", netmask: "255.255.255.0", nic_type: '82545EM'
  
  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    # vb.gui = true
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    #vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
    #eth2
    vb.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
    #vb.customize ["modifyvm", :id, "--nicpromisc4", "allow-all"]
  end
end

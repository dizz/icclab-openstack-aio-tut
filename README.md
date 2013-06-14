# README

This is a simple vagrant file that will allow you start an empty virtual machine to begin the manual install of OpenStack Grizzly. 

The guide used for the installation is [@mseknibilel](https://github.com/mseknibilel/OpenStack-Grizzly-Install-Guide/blob/OVS_SingleNode/OpenStack_Grizzly_Install_Guide.rst) single node guide.

## Notes
The virtual machine will be configured with three network adapters. The first (`eth0`) is used by vagrant. The second (`eth1`) is an adapter used for internal management of OpenStack. The third (`eth2`) is the public interface both for in-bound and out-bound traffic. In order for out-bound traffic to work, incoming traffic on `eth2` is redirected, using `iptables`, to `eth0`. 

To have your VMs ping google (e.g. 8.8.8.8), you need to carry out some additional configuration when installing OpenStack. These steps are not in the main guide noted above (they are as a pull request), but just in case here they are:


### 5.1.1. OpenVSwitch (Part2, Optional)


* This will guide you to setting up the `br-ex` interface. Edit the `eth1` in `/etc/network/interfaces` to become like this:

      # VM internet Access 
      auto eth1 
      iface eth1 inet manual 
      up ifconfig $IFACE 0.0.0.0 up 
      up ip link set $IFACE promisc on 
      down ip link set $IFACE promisc off 
      down ifconfig $IFACE down 

* Add the `eth1` to the `br-ex`:

      #Internet connectivity will be lost after this step but this won't affect OpenStack's work
      ovs-vsctl add-port br-ex eth1

* You will want to get the internet connection back, so assign the `eth1` IP address to the `br-ex` in the `/etc/network/interfaces` file::

      auto br-ex
      iface br-ex inet static
      address 192.168.100.51
      netmask 255.255.255.0
      gateway 192.168.100.1
      dns-nameservers 8.8.8.8

* Note to [VirtualBox](http://www.virtualbox.org) users, you will likely be using host-only adapters for the private networking. You need to provide a route out of the host-only network to contact the outside world; egress is not supported by host-only adapters. This can be done by routing traffic from `br-ex` to an additional NAT'ed adapter that you can add. Run these commands (where NAT'ed adapter is `eth0`)::

      iptables --table nat --append POSTROUTING --out-interface `eth0` -j MASQUERADE
      iptables --append FORWARD --in-interface br-ex -j ACCEPT

To create the quantum external network you should then follow the [multinode guide's section 5](https://github.com/mseknibilel/OpenStack-Grizzly-Install-Guide/blob/OVS_MultiNode/OpenStack_Grizzly_Install_Guide.rst#5-your-first-vm) on this. Note: when creating the external network, be sure to set the gateway IP to `192.168.100.51`.

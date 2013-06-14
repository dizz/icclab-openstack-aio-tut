# README

This is a simple vagrant file that will allow you start an empty virtual machine to begin the manual install of OpenStack Grizzly. 

The guide used for the installation is [@mseknibilel](https://github.com/mseknibilel/OpenStack-Grizzly-Install-Guide/blob/OVS_SingleNode/OpenStack_Grizzly_Install_Guide.rst) single node guide.

## Notes
The virtual machine will be configured with three network adapters. The first (`eth0`) is used by vagrant. The second (`eth1`) is an adapter used for internal management of OpenStack. The third (`eth2`) is the public interface both for in-bound and out-bound traffic. In order for out-bound traffic to work, incoming traffic on `eth2` is redirected, using `iptables`, to `eth0`. 
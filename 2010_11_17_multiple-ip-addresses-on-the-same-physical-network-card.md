Title: Multiple IP addresses on the same physical network card
Date: 2010-11-17 12:54
Tags: technical
Category: Technical Solutions
Slug: multiple-ip-addresses-on-the-same-physical-network-card
Summary: A quick walkthrough on how to configure a single network card to pull multiple IP addresses (RedHat based distribution)
Status: published

There are times when a server can be allocated more than one IP Address even though it contains only one physical 
network card. To associate these IP addresses with the server some manipulation of networking settings will need to be 
performed. The steps outlined in this walk-through are for RedHat based systems. This tutorial is for statically assigned 
IP Addresses (as a server generally will have).
For this walk through we are going to add one additional IP address to `eth0`. Navigate to

    cd /etc/sysconfig/network-scripts

Copy `ifcfg-eth0` to `ifcfg-eth0:0`

    cp ifcfg-eth0 ifcfg-eth0:0

Now we need to modify the new file slightly so that it gets it's own IP address. Open `ifcfg-eth0:0` in your favorite editor

	DEVICE=eth0:0     		<-- Change this to match the new eth0:0 file we just created
	BOOTPROTO=none
	BROADCAST=x.x.x.x   	<-- This is the broad cast address for the subnet the new IP is on
	DNS1=x.x.x.x    		<-- This is the main DNS server you are using (example: 64.120.14.26)
	GATEWAY=x.x.x.x   		<-- This is the gateway address for the subnet the new IP is on
	HWADDR=<DO NOT CHANGE>  <-- Don't change this from what is existing. The Hardware address is the same as the physical one
	IPADDR=x.x.x.x   		<-- This is your new IP address
	NETMASK=x.x.x.x   		<-- This is the netmask for the subnet the new IP is on
	ONBOOT=yes    			<-- Leave to yes
	OPTIONS=layer2=1
	TYPE=Ethernet
	PREFIX=29
	DEFROUTE=yes
	NAME="System eth0:0"    <-- Change to reflect new name of device

Save your file with the new settings. Now we need to restart the networking service:

    service network restart

When the network components come back up you should see your new device in the `ifconfig` command. To add more IPs, 
copy and replace values as specified above.


# OpenWrt Installation
1. Use ipconfig command in windows || ifconfig -a in mac find your ip address and your gate address
   
   <sub>If there is ip address in 192.168.1.x, the gateway is potentially 192.168.1.1</sub>
3. Login to the gateway original firmware with default username and password
4. Download the firmware on https://openwrt.org/toh/views/toh_fwdownload
5. Go to the default gateway and find advance setting where allow you to install firmware and install it with the image you have just download
6. When the installation is complete, you can check on ipconfig again and see if you have a local subnet, and that is potentially 192.168.1.x
7. Go to the openWrt web interface via 192.168.1.1

![image](https://github.com/RyanLan0213/Infrastructure/assets/89798274/e9f7d23b-8c52-4be3-91f8-49fef146baa1)



## Suppose we have the above architecture of network and we are parent router as ParentRouter, two child router as ChildA and ChildB.
### Router info example spec:
**ParentRouter:**
ip:192.168.1.1
WAN:10.0.0.20 (Assigned by ISP/main router)


**Child A:**
ip:192.168.1.1
WAN:192.168.1.2 (Assigned by ISP/main router)


**Child B:**
ip:192.168.1.1
WAN:192.168.1.3 (Assigned by ISP/main router)


## Set up in ParentRouter:
**Change LAN subnet**
1. Go to network -> Interface in the top left corner.
2. Find the LAN we are currently using, and click Edit
3. Change the static ip address to the subnet that you want. In example set up, this will be 192.168.160.1
4. When the router reboot, the WAN for child A will be changed to 192.168.160.x and WAN for child B will be changed to 192.168.160.x as well.

**Set up static route**
1. Go to Network -> Routing
2. Add route for ipv4
3. Add static route for router A:
   _interface:LAN_
   _destination:ChildA's ip address_
   _gateway:ChildA's WAN address_
4. Add static route for router B:
   _interface:LAN_
   _destination:ChildB's ip address_
   _gateway:ChildB's WAN address_
This allow any request to child subnet to be redirect to correct child router through WAN address.
For example if ChildA wants to access ChildB, ChildA will reach parentRouter first and then route to ChildB's router through ChildB's WAN address


## Set up in ChildA:
**Change LAN subnet**
1. Go to network -> Interface in the top left corner.
2. Find the LAN we are currently using, and click Edit
3. Change the static ip address to the subnet that you want. In example set up, this will be 192.168.166.1
4. reboot

**Set up static route**
1. Go to Network -> Routing
2. Add route for ipv4
3. Add static route for router A:
   _interface:WAN_
   _destination:ChildB's ip address_
   _gateway:parentRouter gateway(192.168.160.1)_

This step allows router A able to route to router B by first going to parentRouter

**Allow ICMP and TCP in traffic rule**
1. Go to Network -> firewall -> Traffic Rules
2. Add ipv4 for ICMP rule for forward and input to ACCEPT
3. Add traffic rule to ACCEPT TCP from WAN interface to LAN interface. WAN(192.168.160.1) to LAN port 443(HTTPS)

**Allow Firewall Zone from WAN to LAN**
1. Go to Network -> firewall -> firewall zone
2. Set WAN to LAN to be accept(Input), accept(Output), accept(forward)
This will allow the router able to access to local LAN in web interface.

## Set up in ChildB:
**Change LAN subnet**
1. Go to network -> Interface in the top left corner.
2. Find the LAN we are currently using, and click Edit
3. Change the static ip address to the subnet that you want. In example set up, this will be 192.168.168.1
4. reboot

**Set up static route**
1. Go to Network -> Routing
2. Add route for ipv4
3. Add static route for router A:
   _interface:WAN_
   _destination:ChildA's ip address_
   _gateway:parentRouter gateway(192.168.160.1)_

This step allows router B able to route to router A by first going to parentRouter

**Allow ICMP and TCP in traffic rule**
1. Go to Network -> firewall -> Traffic Rules
2. Add ipv4 for ICMP rule for forward and input to ACCEPT
3. Add traffic rule to ACCEPT TCP from WAN interface to LAN interface. WAN(192.168.160.1) to LAN port 443(HTTPS)

**Allow Firewall Zone from WAN to LAN**
1. Go to Network -> firewall -> firewall zone
2. Set WAN to LAN to be accept(Input), accept(Output), accept(forward)
This will allow the router able to access to local LAN in web interface.



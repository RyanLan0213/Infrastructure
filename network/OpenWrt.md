## OpenWrt Installation
1. use ipconfig command in windows || ifconfig -a in mac find your ip address and your gate address
   <sub>If there is ip address in 192.168.1.x, the gateway is potentially 192.168.1.1</sub>
2. Login to the gateway original firmware with default username and password
3. Download the firmware on https://openwrt.org/toh/views/toh_fwdownload
4. Go to the default gateway and find advance setting where allow you to install firmware and install it with the image you have just download
5. When the installation is complete, you can check on ipconfig again and see if you have a local subnet, and that is potentially 192.168.1.x
6. Go to the openWrt web interface via 192.168.1.1


Suppose we have the above architecture of network and we are parent router as ParentRouter, two child router as ChildA and ChildB.


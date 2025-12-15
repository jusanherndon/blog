As I mentioned in my home\ lab.md file, I am using a [Ubiquiti EdgeSwitch 5XP](https://store.ui.com/us/en/category/wired-edge-max-switching/products/es-5xp) in my home lab.

I am including a drawling in this folder to give a good example of how my home lab topology looks.


Here are the devices I have in my network:

# ISP Connection
  I have an ATT&T Fiber gateway, setup in **IP Passthrough** to allow for it to convert a LAN (Local Area Network) port into a WAN (Wide Area Network) port. This is useful because it allows me to give a WAN ip instead of a LAN ip to my own hardware on the network. 

  I also disabled any wireless radios on the gateway to prevent any devices from connecting to it, instead of my other router. 

  Lastly, I changed the routing subnet to another private /24 network to avoid address collisions on my network, and to allow me to still access the ATT&T gateway from my own Router I set up.

# My own Router

  I am using a pair of devices for the main part of my network. It is a gateway and an extending AP for the upstairs of my house. These two devices are probably the least complicated part of my network.

# Lastly, the Ubiquiti EdgeSwitch

  Just to explain why I wanted a manged switch/router is because the AP I am using above only has one ethernet port and I have multiple devices that I want to use on Ethernet connections rather than wifi.
 

  I had a lot of trouble when I first setup this device. This switch made router through Openwrt was strange. I knew to change the routing subnet to another private /24 network, but I was still getting incorrect ips from the device. Then I started looking through the [luci](https://github.com/openwrt/luci) gui and found a setting called **Authoritative DHCP server**.

  This basically means that you want this device to not host its own DHCP server and their is another DHCP server that should handle giving out addresses. I also realized that the AP I am using above also uses this setting in order to use the main Router's DHCP server.

  Which is really useful when you have multiple devices handling networking, like I do. I was not able to setup a management ip for the EdgeSwitch, but everything else is working fine.

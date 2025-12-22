# Motivation

Routing is extremly complex and it would be nice to be able to not have to trust open WiFi networks and instead have my own router to avoid untrusted networks.

A perfect example of this was when I vacationed at a Disney Resort. WiFi in a lot of places is usually neglected. On the hotel's router, no encryption [scheme](https://openwrt.org/docs/guide-user/network/wifi/encryption) was set at all. Which is a bad idea, because that means all WiFi packets between you device and Router are unencrypted. So all an attacker has to do is get a packet capture of the router and then [SSL or TLS](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/) could be broken and all of your data could be stolen.

Having a travel router on any encryption would help, because the packets would be encrypted from your router onwards. So it would be harder for the attacker to steal your info. In this specific case, you are still kinda SOL if you are using any wireless connection to the hotel router and I would advise trying to set up an ethernet connection to the router, if you have to. It would be a lot safer.

# Requirements
- Ability to connect to a network either wirelessly or wired
- Ability to connect to my current Tailscale network and allow other devices to do so freely
    - [OpenWrt Tailscale page](https://openwrt.org/docs/guide-user/services/vpn/tailscale/start) 
- Make a nice kit for it to fit into a small travel bag
- Be low power enough, so a power bank could power it
- Be able to work in every country, since a lot of countries have different Wireless Band Laws

# Router I am going to use
I chose the [OpenWrt One](https://openwrt.org/toh/openwrt/one?s[]=openwrt), since I already own one and it is a small WiFi 6 device.

Also OpenWrt has [Wireless regdb](https://git.kernel.org/pub/scm/linux/kernel/git/wens/wireless-regdb.git/tree/db.txt?id=HEAD) built into the OS, because OpenWrt would like their routers to work in every country.

# Problems Encountered

I am not sure how to do an WWAN setup on OpenWrt. As in having a wireless connection be my WAN instead of a wired one.



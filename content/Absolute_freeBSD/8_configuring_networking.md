+++

Title = "Configuring Networking"
weight = 7

+++

#### Network Prerequisites

If your network is DHCP(Dynamic Host Configuration Protocol), you can connect to the network as a client without knowing anything about the network. A static IP address makes more sense on a server.

Both IPv4 and IPv6 require the following information :

- An IP Address
- The netmask for that IP address and protocol
- The IP address of the default gateway

use `ifconfig(8)` and `route(8)` to attach system to network and make configuration permanent in `/etc/rc.conf`

### Configuring Changes with ifconfig(8)

list all interfaces by using `ifconfig(8)` command without any arguments

![image-20210910074858189](/8_configuring_networking.assets/image-20210910074858189.png)

First network interface is em0 , or the first network card that uses `em(4)` driver. The `em(4)` page reveals that this is an Intel PRO/1000 card. Point:2 shows its state i.e. UP meaning its either working or trying to work. Its assigned an IPv4 address along with a netmask (point 3). This card has 2 IPv6 addresses, the link-local address(begins with fe80) and global address. You'll see MAC address (6) and connection speed and finally status entry shows that this card is active : a cable is plugged in and we have a link light.

The second card `rl0` has almost none of this information associated with it. One key face is that `no carrier` signal : it's not plugged in and there is no link light. Card not in use.

Finally we have interface lo0, the loopback. This interface has IPv4 address 127.0.0.1 and IPv6 address ::1 on every machine. This is used when machine talks to itself.This is a standard software interface, which does not have any associated physical hardware. Do not attempt to delete or change its IP address- things will break in an amusing way if you do so.

### Adding an IP to an Interface

install process will configure any network cards you have working at install time. If you didn't configure network during setup process, or if you add/remove cards after install, you can assign IP address to your network card with `ifconfig`

`ifconfig interface-name inet IP-address netmask`

`ifconfig em0 inet 203.0.113.250 255.255.255.0` specify the netmask in dotted or hex notation

or more simpler :) to use slash notation`ifconfig em0 inet 203.0.113.250/24`

for IPv6 `inet6`

The `ifconfig(8)` can also perform any other configuration your network cards require, letting you work around hardware bugs in features such as various sorts of checksum offloading like setting media type and duplex mode for sub-gigabit interfaces.

example : disables checksum offloading and TCP segmentation

`ifconfig em0 inet 203.0.113.250/24 -tso -rxcsum`

To make this persist across reboots, add an entry to `/etc/rc.conf` that tell system to configure the card at boot.

example configuring rl0 card

```
ifconfig_rl0 = "inet 203.0.113.250/24"
ifconfig_rl0_ipv6 = "2001:db8::bad:c0de:cafe/64"
```

Once you have working config just copy it over to /etc/rc.conf entry.

### Testing you interface

Now interface has IP address, try to ping the IPv4 address of your default gateway. If you get a response, you're on the local network.

for IPv4 use `ping(8)` , while for IPv6 use `ping6(8)`

![image-20210910102842399](/8_configuring_networking.assets/image-20210910102842399.png)

### Set Default Route

default route is the address where your system sends all traffic that's not on the local network. If you can ping the default route's IPv4 address, set it via `route(8)`

```
route add default 203.0.113.1
```

That's  it ! You should now be able to ping any IPv4 address on the internet.

```
route -6 add default 2001:db8::1
```

If you have not used nameservers during system install, you'll have to use the IP address rather than the hostname. Once you have a default router, make it persist across reboots by adding proper `defaultrouter` and `ipv6_defaultrouter` entries in /etc/rc.conf.

### Multiple IP Addresses on One Interface

A FreeBSD system can respond to multiple IP addresses on one interface. This is especially useful for jails. Note : netmask on IPv4 alias is always /32, regardless of size of network address block the main address uses. IPv6 alias uses the actual prefix length (slash) of the subnet they are on.

```
ifconfig em0 inet alias 203.0.113.225/32
```

```
ifconfig em0 inet6 alias 2001:db8::bad:c0de:caff/64
```

So when aliases are added main IP always appears first when you do `ifconfig`.

We can persist alias reboots by adding additional `ifconfig` statements in `/etc/rc.conf`

````
ifconfig_em0_alias0="inet 203.0.113.225/32"
ifconfig_em0_alias1="inet6 2001:db8::bad:c0de:caff/64"
````

Note only difference in this entry and previous entry are the alias chucks with a unique number.

#### Renaming Interfaces

Note while FreeBSD is flexible on interface names, some software isn't - it assumes that a network interface name is short word followed by a number like eth0 , eth1, etc.

Ex : to rename em1 to test1

```
ifconfig em1 name test1
```

To persist this change use `/etc/rc.conf`

```
ifconfig_em1_name="test1"
```

Note : FreeBSD renames interfaces early in the boot process, even before IPs are setup. This means that any further interface configuration must reference to new name rather than old one.

````
ifconfig_em1_name="dmz2"
ifconfig_dmz2="inet 203.0.113.2 netmask 255.255.255.0"
ifconfig_dmz2_alias0="inet 203.0.113.3"
````

#### DHCP

Very few networks use DHCP for everything, including servers. It sets up everything for you. If your Network Admin configures servers via DHCP, you can tell the network card to take its configuration via DHCP with the following

```
ifconfig_em0="DHCP"
```

#### Reboot!

Note : always be careful these setting if you have configured things incorrectly then you can't access your server. If you feel like living dangerously , you can run `service netif restart` with interface name to configure only a single interface.

`service netif restart em0`

### The Domain Name Service (DNS)

DNS provide a map between hostnames and IP addresses. It also provides the reverse map, of IP addresses to hostnames.

A host that trawls the internet to dig up DNS mappings is called as name server, or DNS server. *Authoritative* nameservers provide DNS mappings for the public to find an organization's nameservers. 

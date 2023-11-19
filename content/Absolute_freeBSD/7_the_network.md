+++

Title = "The Network"
weight = 7

+++

FreeBSD is famous for its network performance. The TCP/IP network protocol was first developed on BSD, and BSD, in turn, included first major implementation of TCP/IP. The wide availability, flexibility, and liberal licensing of BSD TCP/IP stack made it the de facto standard.

The dominant internet protocol is TCP/IP. TCP (transmission control Protocol over Internet Protocol). TCP is transport protocol while IP is a network protocol, but they're so tightly intertwined that they're generally referred to as a single entity.

### Network Layers

Each layer handles a specific task and can communicate with a layer above and below it. The classic OSI (Open System Interconnections) network protocol stack has seven layers, is exhaustively complete, and covers almost any situation.

But we will limit discussion to TCP/IP networks, such as internet and almost all corporate networks, so we need consider only four layers of network stack.

**The Physical Layer**

At the very bottom is physical layer : the network card and the wire, fiber, or radio waves leaving it. This layer includes the physical switch; the hub or base station; the cable attaching to router; and fiber that runs from your office to the telephone company. It's as simple as having intact hardware.

One of the functions of internet routers is to connect one sort of physical layer to another - for eg ethernet to optical fibre. 

**Datalink : The physical protocol**

This layer transforms information into the actual ones and zeros that are sent over the physical layer in the appropriate encoding for that physical protocol.

For eg IP version 4 (IPv4) over Ethernet uses Media Access Control (MAC) addresses and the Address Resolution Protocol (ARP); IP version 6 (IPv6) over Ethernet uses Neighbor Discovery Protocol (NDP or ND). 

In addition to exchanging information with the physical layer, the datalink layer communicates with the network layer.

**The Network Layer**

It maps connectivity between network nodes, answering questions like "Where are other hosts?" and "Can you reach this particular host?" This logical protocol provides a consistent interface to programs that run over the network, no matter what sort of physical layer you're using.

Network Layer used on internet is IP. IP provides each host with a unique address known as IP Address, so that any other host on the network can find it.

**Heavy Lifting : The Transport Layer**

The three common transport layer protocols are ICMP, TCP, and UDP.

ICMP (Internet Control Message Protocol) manages basic connectivity messages between hosts with IP addresses.

The UDP (user datagram Protocol) and TCP.

These provide services such as multiplexing via port numbers and transmitting user data. UDP is bare-bones transport protocol offering the minimum services needed to transfer data over the network. TCP provides more sophisticated features, such as congestion control and integrity checking.

In addition to these three, many other protocols run above IP. The files `/etc/protocols` contain a fairly comprehensive list of transport protocols that use IP as an underlying mechanism.

**Application**

Application are part of the network. Application open requests for network connectivity, send data over the network, recieve data from the network, and process that data.

### The Network in Practice

Suppose a user connected to internet via your network wants to look at Google ! The user accesses his web browser and enters URL. The browser knows how to talk to next layer down in the network, which is transport layer. After kneading the user's request in appropriate form, the browser asks the transport layer for a TCP connection to a particular IP address on port 80. (we skipped DNS resolution etc. )

The transport layers examines the request, requested TCP connections get allocated appropriate system resources for that sort of connection. The request broken up into digestible chunks and handed down to network layer.

The network layer doesn't care about actual request. Its been handed some data which need to read to its location. It bundles TCP data with proper addressing information. The resulting mess is *packet*. The network layers hands these packets down to datalink layer.

Datalink doesn't care about the contents of the packet. It certainly doesn't care about IP addressing or routing. Its been given lumps of zeros and ones and its job is transmitting them. Datalink layer may add proper header/footer information to packets called as *frame*. Finally frame is handed off to physical layer for transmission through wires,wave or other media.

Physical layer has no intelligence at all, it just transmits whats given to it.

When routers receives zeros and ones, it hands them up to datalink layer. The datalink layer strips its framing information and hands the resulting packet up to network layer within the router. The router's network layer examines the packet and decides what to do with it based on its routing tables. It then hands the packet to datalink layer. This might be another Ethernet interface or perhaps a PPP interface out of a T1.

Due to layering abstraction,  neither your computer nor your user need to know anything about any of these.

When the request reaches its destination, the computer at the other end of transaction accepts the frame and sends it all the way backup the network stack.

### Getting Bits and Hexes

As a sys admin you will come across terms like *48-bits address* and *18-bit netmask*. Note hexadecimal digits are 4 bits long.

Note : hexadecimal is 0-15 for higher bits it uses characters and is often represented as `0x`.

### Network Stack

Network stack is  the software that lets a host communicate with other hosts over the network. A host can run with a IPv4 only network stack, an IPv6-only network stack or dual-stacked setup. FreeBSD enable both by default.

### IPv4 Addresses and Netmasks

IP Address is a unique 32-bit number assigned to a particular node on a network. Some IP addresses are more or less permanent, such as those assigned to vital servers.

32-bit number is broken into 4 8-bit blocks. for eg 203.0.113.1

A *netmask*, which also be called a *prefix length* or *slash*, is a label indicating the size of block of IP addresses assigned to your local network. Your IP block determines netmask aka how many IP addresses you have.

When you get a block of IP addresses for your server, it'll look like this 203.0.113.128/25. Think of IP addresses as string of binary number. You can change the bits on the far right, but not on the far left.

 Now changing the bits is not limited to last 8 bits. The /25 indicates that 25 bits are fixed rest 7 bits can be changed. You get a decimal netmask by seeting the fixed bits to 1 and your network bits to 0.

```
11111111.11111111.11111111.10000000
```

**Computing Netmasks in Decimal**

You want to find how many IP addresses you have on your network. This will be a multiple of 2 almost certainly smaller than 256. Subtract the number of IP addresses you have from 256. This is last number of your netmask. If you IP address is 203.0.113.100/26, you'll need to know that /26 is 26 fixed bits, or 64 IP addressess. Look at your last IP address, 100. It certainly isn't between 0 and 63, but its between 64 and 127. The other hosts on your IP block have IP addresses ranging from 203.0.113.64 to 203.0.113.127

and netmask is 255.255.255.192 (256-64).

 ![image-20210815071543687](/7_the_network.assets/image-20210815071543687.png)

**Usable IP Addresses**

You can't use all IP addresses in a block. First IP address in a block is *network number*, which is used for internal bookkeeping.

Traditionally, the last number in any block of IP address is called as *broadcast address*. According to original IP specification, every machines on a network was supposed to respond to a request for this address. This allowed you to ping the broadcast address to quickly determine which IP addresses were in use.

It was transformed into an attack technique and was disabled by default on almost all OS and network appliance.

**Assigning IPv4 Addresses**

Every Network *interface* has IP address. most computers have only one network interface, so for them, the difference is nonexistent. If you many cards , each has different IP address. On the other hand with special configuration you can bond multiple cards into a single network interface, giving one computer one virtual interface despite many cards. Remember 127.0.0.1 is always attached to every host's loopback interface. It can only be reached from the local machine.

### IPv6 Addresses and Subnets

The original engineers of IPv4 thought that 4.29 billion IP addresses would be enough for the whole world. Computers were expensive, after all, and only military and educational system connected to internet.

Now there is quite a demand for IPv4 and it is getting expensive and that is why IPv6 was discovered.

**IPv6 Basics**

IPv6 is a network-layer protocol. TCP, UDP, ICMP, and other protocols run atop it. Note IPv6 uses 128 bit address, expressed as eight groups of four hexadecimal characters, separated by colons.

Its quite huge in magnitude just to get the idea count all the humans ever lived and count their cells and not only their cells but also the bacterial cells in their bodies.

**Understanding IPv6 Addresses**

As with decimal IP Addresses we don't need to display leading zeros. `2001:db8:5c00:0:90ff:bad:c0de:cafe` could be written as `2001:0db8:5c00:0000:90ff:0bad:c0de:cafe`. How do you differentiate ports colon :D

`[2001:db8:5c00:0:90ff:bad:c0de:cafe]:80` xD

**IPv6 Subnets**

Obvious and natural ways to divide the network are /16, /32, /48, /64, /80, /112. 

ISPs are usually issues a /32 or /48 and are expected to issue end-user networks, such as a typical clients, a /64 network. When you subnet at 16-bit boundaries, each network has 65,536 subnets of the next smaller size. 

Tip : Search on net IPv6 subnet calculator.

**Link-Local Address**

Addresses beginning with `fe8x:` are local to their interface. Every interface has such link-local address that are valid on a specific local network. Even if an IPv6 network has no router, hosts on the local directly attached network can find each other and communicate using these local address. These are /64 subnets.

**Assigning IPv6 Addresses**

IPv6 clients on a /64 or larger network can normally autoconfigure their network through *router discovery*. Router discovery resembles a stripped down DHCP service. The router broadcasts gateway and subnet information, and hosts configure to use it.

Modern version of router discovery include very basic DHCP-style options such as DNS servers. Not all IPv6 providers include these options in their router discovery configuration.

Servers should not use IPv6 autoconfiguration. A server usually needs a static IP, even in IPv6. Hosts on a network smaller /64 must be manually configured.

The address ::1 always represents the local host and is assigned to the loopback address.

### TCP/IP Basics

**ICMP**

The Internet Control Message Protocol (ICMP) is the standard for transmitting routing and availability messages across the network. Tools such as `ping(8)` and `tracerouter(8)` use ICMP to gather their results. ICMP is different for IPv4 and IPv6.

Some people claim you must block ICMP for security reasons. Proper IPv4 network performance require large chunks of ICMPv4. If you feel you must block ICMP, do so selectively.

IPv6 dies without ICMPv6, as IPv6 doesn't support packet framentation. If you use IPv6 never block ICMPv6.

**UDP**

User Datagram Protocol (UDP) is the most bare-bones data transfer protocol that runs over IP. It has no error handling, minimal integrity verification, and no defense against data loss.

When host transmits data via UDP, the sender has no way of knowing whether the data ever reached its destination. we can't verify the source of data-while UDP packet includes source address, this can be easily faked. That is why UDP is called connectionless, or stateless.

Applications that use UDP have their own error-correction handling methods that don't mest well with the defaults provided by protocols such TCP.

UDP is also a *datagram* protocol, meaning that each network transmission is complete, self-contained, and received as a single integral unit.

**TCP**

TCP includes nifty error correction and recovery. The receiver must acknowledge the received packet other otherwise sender retransmits those packets.

TCP is also a *streaming* protocol, meaning that a single request can be split among several packets and recipient could receive them out of order and need to order them correctly.

For two host to exchange TCP data, they must set up a channel for that data to flow across. One host requests a connection, the other host responds to request and then first host starts transmitting. This is called as a *three way handshake*.

**How Protocols Fit Together**

The datalink layer(ARP, in case of IPv4 over Ethernet) lets you see everyone else at the table. IP gives every person unique chair, expect for three young nephews using bench NAT. ICMP provides basic routing information. TCP confirms integrity. UDP is for sending or throwing  lots of data :D

**Transport Protocol Ports**

When a network server program starts, it attaches or binds, to one or more logical ports. A logical port is just arbitrary number ranging from 1 to 65535. For example internet mail server bind to TCP port 25.

Each TCP or UDP packet arriving has a field indicating its desired port. Each incoming request is flagged with a desired destination port number. This means other programs can run on different ports, client can talk to those different ports and nobody except the sysadmin gets confused :)

The `/etc/services` file contains list of port numbers and the services that they're commonly associated with. The file has a very simple five-column format

```
qotd	17/tcp	quote	#Quote of the Day
```

This is entry for qotd service which runs on port 17 in the TCP protocol. It's also known as `quote` service and comment provides more details.

**Reserved Ports**

Ports below 1024 in both TCP and UDP are called *reserved ports*. These ports are assigned to core internet infrastructure and important services such as DNS, SSH, HTTP, LDAP and so on.\

You can change the reserved ports with the sysctls `net.inet.ip.portrange.reservedhigh` and `net.inet.portrange.reservedlow`

### Understanding Ethernet

Ethernet is a shared network; many different machines can connect to the same Ethernet and can communicate directly with each other. This gives Ethernet a great advantage over other protocols, but Ethernet has physical distance limitations.

**Protocol and Hardware**

Ethernet is a broadcast protocol, which means that every packet you send on the network can be sent to every workstation on the network. Either you network card or its device driver separates the data intended for your computer from the data meant for other computers. One side effect is eavesdropping can be pain for network traffic. While this can be very useful when diagnosing problems, it's also a security issue. Capturing clear-text passwords is trivial on an old-fashioned Ethernet.

An Ethernet *hub* is a central piece of hardware to physically connect many other Ethernet devices. Hubs simply forward all received Ethernet frames to every other device attached to network.

**Switch Failure**

**Ethernet Speed**

**MAC Addresses**


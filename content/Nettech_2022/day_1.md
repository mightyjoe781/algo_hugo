+++

Title = "Day1"
chapter = true

+++

# Day1

Every computer/server/router/mobile on a network has unique Physical Address - MAC Address.

MAC : Media Access Control is a 6 byte hexadecimal. e.g. `a1:00:ff:12:bc:de` which can't be changed because its permanently embedded on network card.

On windows type : `ipconfig /all` and on macOS/linux machines just type : `ifconfig`.

Note : you can't change MAC address, all the different tools available just hide/mask your original MAC address. MAC address is accessible only on local network.

MAC address can't do routing and it works only on its own network.

Logical Address : IP address is more suitable and useful for routing. There are 2 naming schemes : ipv4 and ipv6. As a result its logical address, its temporary and it may change when you log out from network. This is automatically assigned by DHCP server.

ipv4 : 32 bit address, while ipv6 : 128 bit address.

Every IP address comes with a subnet mask i.e. 255.255.255.0 

localhost address/loopback address : 127.0.0.1

# Day2

Class of IP address : say 192.168.1.100

- 0-127     - A class address - N.H.H.H
- 128-191 - B class address - N.N.H.H
- 192-223 - C class address - N.N.N.H

- network address is - 192.168.1.0 (first address)
- host address is - 192.168.1.100
- broadcast - 192.168.1.255 (last address)

Effectively you have network-2 addresses

Another example - B Class network 150.1.1.100

- na - 150.1.0.0
- ba - 150.1.255.255

Concept of Private IP Address
All the IPs are hidden by router so public IP address is of the router.

demo exam

- 210.100.3.200/23
- 150.100.21.22/11
- 201.100.32.100/28
- 160.23.19.200/20
- 203.167.123.90/21

## 😀Day 3

layers - 7

- 7 - Application -
  - http / https / telnet ftp / dns / dhcp / imap / smtp / pop3 / ssh
- 6 - Presentation - 
  - 
- 5 - Session
  - 
- 4 - Transport (TCP / UDP)
  - Transmission Control Protocol
  - User Datagram Protocol
- 3 - Network ( IP Address, ARP, RARP, ICMP, IGMP)
  - Routers, Switch
- 2 - Data Link - MAC/LLC - Mac Address
- 1 - Physical (layer - 1) - HUB



VLSM - Variable length subnet mask

use descending order

202.1.1.0/24

- Dlink - 44 : 64  -       202.1.1.0/26
- BITS Goa - 15 : 32 - 202.1.1.64/26
- Dabolim - 40 : 64 -  202.1.1.128/27
- Novotel - 12 : 16 -   202.1.1.160/27
- Queeny - 10 : 16 -   202.1.1.192/28
- IIT Goa - 30 : 32 -    202.1.1.208/28
- Saritas - 6 : 8.      -    202.1.1.224/29

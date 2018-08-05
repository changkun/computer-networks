# Network Address Translation (NAT)

We know that:

- IP addresses are used for end-to-end addressing
- They are globally unique for this reason
- especially the IPv4 addresses that are mainly used today and very scarce.

Question: Do IP addresses always have to be unique?

Answer: 

- No, IP addresses do not have to be unique if:
  + no communication with between located on the Internet 
  + the non-unique private IP addresses are translated appropriately into public addresses.


Definition: NAT

Network Address Translation (NAT) is generally referred to as techniques that allow `N` private (non-globally unique) IP addresses to be mapped to `M` global (globally unique) IP address.

- `N <= M`: The translation is done statically or dynamically by assigning at least one public IP address to each private IP address.
- `N > M`: In this case, a public IP address will be shared between multiple computers. A clear distinction can be achieved by means of port multiplexing. The most common case is `M = 1`, e.g. in private DSL connection.

What are private IP addresses?

Private IP addresses are special address ranges which 

- are released for private use without prior registration
- can be therefore occur in different networks
- For this reason, not clear and for end-to-end addressing between publicly available networks
- Therefore IP packets with private recipient addresses not forwarded by routers on the Internet (or should be)

The private address ranges are:

- 10.0.0.0/8
- 172.16.0.0/12
- 169.254.0.0/16
- 192.168.0.0/16

The range 169.254.0.0/16 is used for automatic address assignment (Automatic Private IP addressing):

- If a computer starts without a statically assigned address, it attempts to reach a DHCP server.
- If no DHCP server can be found, the operating system assigns a random address from this address block.
- If the ARP resolution to this address fails then it is assumed that this address is not yet used on the local subnet. Otherwise, another address is selected and the process is repeated.

How doesn NAT work in detail? Example:

Usually, routers assume the network address translation:

```
    PC1     --------+
192.168.1.1         |         IF1: 192.168.1.254
                 Switch ------------ Router ------------ Internet ------- Server
                    |         IF2: 131.159.20.19                      185.86.235.241
    PC2     --------+
192.168.1.2
```

- PC1, PC2 and Router can communicate with each other via private IP addresses in the subnet 192.168.1.0/24.
- R can be reached globally via its public address 131.159.20.19.
- PC1 and PC2 cannot communicate directly with other hosts on the Internet because of their private address.
- Hosts on the Internet can not reach either PC1 or PC2 even if they know PC1 and PC2 are behind Router and Router's global address is known.

PC1 accesses a web page located on the server with the IP address 185.86.235.241.

The NAT table of R is empty at the beginning:

| Local IP Addr | Local Port | Global Port |
|:-------------:|:----------:|:-----------:|
|               |            |             |
|               |            |             |

PC1 sends a packet (TCP SYN) to the server:

| SrcIPAddr | 192.168.1.1    |
|:---------:|:--------------:|
| DstIPAddr | 185.86.235.241 |
| SrcPort   | 2736           |
| DstPort   | 80             |

- PC1 uses its private IP address as the sender address
- The source port is randomly selected by PC1 in the range [1024, 65535] (so called **ephemeral ports**)
- The destination is specified by the Application Layer Protocol (80 = HTTP)

Address translation at Router:

| Local IP Addr | Local Port | Global Port |
|:-------------:|:----------:|:-----------:|
|  192.168.1.1  |     2736   |     2736    |
|               |            |             |

- Router exchanges the sender address with its own global address
- If the source port does not cause a collision in the NAT table, it will be retained
- R creates a new entry in his NAT table that documents the changes to the package

| SrcIPAddr | 131.159.20.19  |
|:---------:|:--------------:|
| DstIPAddr | 185.86.235.241 |
| SrcPort   | 2736           |
| DstPort   | 80             |


The server generates a response:

| SrcIPAddr | 185.86.235.241 |
|:---------:|:--------------:|
| DstIPAddr | 131.159.20.19  |
| SrcPort   | 80             |
| DstPort   | 2736           |

- The server does not know about the address translation and considers Router to be PC1
- The recipient address is therefore the public IP address of Router, the destination port is the source port translated by R from the previous message.

Router reverses the address translation:

| SrcIPAddr | 185.86.235.241 |
|:---------:|:--------------:|
| DstIPAddr | 192.168.1.1    |
| SrcPort   | 80             |
| DstPort   | 2736           |

- The NAT table is searched for the destination port number in the column Global Port, which is returned to Local Port. The destination IP address of the packet is exchanged for the private IP address of PC1
- The modified packet is forwarded to PC1
- Like the server, PC1 doesn not know about the address translation.

PC2 now also accesses the server:

- PC2 sends a packet (TCP SYN) to the server:
    - By coincidence, PC2 selects the same source port as PC1 (2736)

| SrcIPAddr | 192.168.1.2    |
|:---------:|:--------------:|
| DstIPAddr | 185.86.235.241 |
| SrcPort   | 2736           |
| DstPort   | 80             |

Address translation to Router:

| Local IP Addr | Local Port | Global Port |
|:-------------:|:----------:|:-----------:|
|  192.168.1.1  |     2736   |     2736    |
|  192.168.1.2  |     2736   |     2737    |

- Router notices that there is already an entry for PC1 for local port 2736
- Router creates a new entry in the NAT table, with a random value chosen for the global port (e.g. the original port of PC2 + 1)
- The package from PC2 is modified accordingly and forwarded to the server

| SrcIPAddr | 131.159.20.19  |
|:---------:|:--------------:|
| DstIPAddr | 185.86.235.241 |
| SrcPort   | 2737           |
| DstPort   | 80             |

From the server's point of view, the "Computer" Router has simply established two TCP connections.

A router could include addtional information in the NAT table:

- destination IP address and destination port
- The protocol used (TCP, UDP)
- Your own global IP address (useful if a router has more than one global IP address)

Depending on the stored information, one differentiates between different types of NAT. The same variant discussed (plus an endorsement of the protocol in the NAT table) is called Full Cone NAT.

Properties of Full Cone NAT:

- FOr incoming connections, the sender IP addresses or the sender port is not checked because the NAT table only contains the destination port and the corresponding IP address or port number in the local network.
- Once an entry exists in the NAT table, an internal host from the Internet can also be reached via this entry for anyone sending a TCP or UDP packet to the correct port number.

Other NAT variants:

- Port Restricted NAT
- Address Restricted NAT
- Port and Address Restricted NAT
- Symmetric NAT

General remarks:

- Firewall allows you to filter incoming and outgoing traffic based on IP addresses, port numbers and previous events ("stateful firewalls"). These funtions are often adopted by routers. In some cases, the content of individual messages also checked ("Deep Packet Inspection"). Then [is NAT a firewall](http://www.internetsociety.org/articles/retrospective-view-nat)?
  + No!
  + Restrictive NAT variants provide basic protection in that they do not allow incoming connections without prior connection from the local network, but this should not be mix-up with the functions of a firewall.
  + Further filtering (as would be the case with a firewall) does not take place.

- How many entries can a NAT table hold?
  + In the simplest case (Full Cone NAT), the theoretical maximum limit is about `2^16` pertransport protocol (TCP and UDP) and per global IP address.
  + For more complex NAT types, adding the destination ports allows more combinations
  + In practice, the size is limited by the capabilities of the router (about 1000 mappings)

- Are mappings deleted from the NAT table?
  + Dynamically generated mappings are deleted after a certain period of inactivity.
  + In some circumstances, a NAT-enabled router may also immediately remove mappings if it deletects a TCP connection loss (implementation-dependent).

- Can entries in the NAT table also be created by hand?
  + Yes, this process is called port forwarding
  + In this way, it becomes possible to behind a NAT one publicly available on a particular port Server to operate.


NAT and ICMP:

- NAT uses port number of the transport protocol
- What if the transport protocol does not have port number or if IP packets without TCP / UDP headers are sent, e.g. ICMP? Answer: The ICMP ID can be used instead of the port number.

Problem: Tranceroute doesn not work with some virtualization solutions. For example, if older versions of Virtualbox NAT implementation are used.

- Tranceroute is based on ICMP TTL Exceeded messages
- These messages have no ICMP ID (unlike an ICMP Echo Reply).
- This is because any IP packet (not necessarily an ICMP Echo Request) can trigger ICMP TTL exceeded, and of course (as in the case of a TCP packet) it doesn not have an ICMP ID.
- Instead, the time-exceeded carries the full IP header and the first 8 bytes of the payload of the packet that triggered the Time-exceeded.
- In the case of TTL-exceeded, a NAT implementation would now have to look for the ICMP ID of an echo request in this first 8 bytes or the port numbers of a transport protocol in order to be able to undo the translation.
- Exactly this back translation is not performed by older versions of Virtualbox's NAT implementation.


NAT and IPv6:

- NAT can also be used for IPv6

Prefix translation

- NAT for IPv6 has specific problems and challenges. RFC6296 specifies IPv6-to-IPv6 Network Previs Translation.
- A 1:1 mapping of addresses is generated.
    - This would also be possible to a limited extent with IPv4 (provided an organization has sufficient public address).
    - But usually not very useful.
- This allows **unique-local unicast addresses** (`fc00::/7`, also a private IPv6 addresses) to be translated into globally valid addresses.
- The translation is done on prefixes:
    - An internal previx `fd01:0203:0405::/48` will be added, e.g., see the global prefix `2001:db8:0001::/48`.
- No Layer 4 features (ports, identifiers) are used.
- The translation is stateless, except for the configuration of the address prefixes. There will be no NAT table needed.
- To prevent the Layer 4 checksums from being modified due to address translation if necessary, the address translation can be chosen so that the original checksums are still correct.


Use of NAT in PVv6

- A common reason for NAT (the address shortage in IPv4) is not present in IPv6.
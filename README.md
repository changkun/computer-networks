# Lecture Notes of Computer Networks

Lecture notes of Computer Networks and Distributed Systems

## Table of Content

- [Chapter 1: Physical Layer]
  + 1.1 Signals, information and thier meaning
    + What are signals?
    + Entropy and information
  + 1.2 Classification of signals
    + Time and frequency range
    + Sampling, reconstruction and quantization
  + 1.3 Transmission channel
    + Influences of the transmission channel on signals
    + Capacity of a transmission channel (model)
  + 1.4 Messaging
    + Source and channel coding
    + Pulse shaping
    + Modulation
  + 1.5 Transmission Media
    + Electromagnetic spectrum
    + Coaxial conductor
    + Twisted pair cable
    + Optical fiber
- [Chapter 2: Link Layer]
  + Representation of networks as graphs
    + Network topologies
    + Adjacency and distance matrix
    + Shortest Path Tree and Minimum Spanning Tree
  + Link Characterization, Multiple Access and Media Access Control
    + Serialization and propagation delays
    + Message Flowcharts
    + ALOHA and Slotted ALOHA
    + CSMA, CSMA/CD and CSMA/CA
    + Token Passing
  + Framing, addressing and error detection
    + Recognition of frame boundaries and code transparency
    + Addressing and error detection
    + Case study: IEEE 802.3u (FastEthernet)
    + Case study: IEEE 802.11a/b/g/n (Wireless LAN)
  + Connections on Layer 1 and Layer 2
    + Hubs, bridges and switches
    + Collision and broadcast domains
- [Chapter 3: Network Layer]
  + Switching modes
    + Circuit switching
    + Messaging
    + Packet switching
  + Addressing on the Internet
    + Internet Protocol version 4 (IPv4)
    + Address Resolution (ARP)
    + Internet Control Message Protocol (ICMP)
    + Address Classes (for Classful Routing)
    + Subnetting and Prefixes (for Classless Routing)
    + Internet Protocol version 6 (IPv6)
    + Stateless Address Autoconfiguration (SLAAC)
    + Internet Control Message Protocol v6 (ICMPv6)
    + Neighbor Discovery Protocol (NDP)
  + Routing
    + Static Routing
    + Longest Prefix Matching
    + Dynamic Routing
    + Algorithms of Bellman-Ford and Dijkstra
    + Routing Protocols (Distance Vector and Link State)
    + Autonomous Systems
- [Chapter 4: Transport Layer](./ch04)
  + [Motivation](./ch04/motivation.md)
  + [Multiplexing](./ch04/4.1-multiplexing.md)
  + [Connectionless transmission: UDP](./ch04/4.2-udp.md)
  + [Connection-oriented transmission: TCP](./ch04/4.3-tcp.md)
    + Sliding Window Protocols (Go-Back-N and Selective Repeat)
    + Transport Control Protocol
    + Flow and Congestion control
  + [Network Address Translation (NAT)](./ch04/4.4-nat.md)
  + [Case Studies](./ch04/4.5-case-studies.md)
    + `SOCK_DGRAM`
    + `SOCK_STREAM`
- [Chapter 5: The Layer 5-7]
  + Layers
    + Pros and cons of different layer models
  + Session Layer
    + Services
    + Functional Units
    + Synchornization
    + Quality of Service
    + Performance Parameters
  + Presentation Layer
    + Data compression (Huffman code)
  + Application Layer
    + Name resolution on the Internet (DNS)
    + HTTP
    + SMTP

# Full Example

- Network A: 10.1.1.0/24
    - router A: 10.1.1.1
    - computer 1 (client): 10.1.1.100
- Network B: 192.168.1.0/24
    - router A: 192.168.1.254
    - router B: 192.168.1.1
- Network C: 172.16.1.0/24
    - router B: 172.16.1.1
    - computer 2 (server): 172.16.1.100:80

Scenario: computer 1 browses 172.16.1.100:80

1) Computer 1: 
- OS knows that it is on network 10.1.1.0/24
- OS decides that 172.16.1.100 is not in own subnet
- configured with gateway 10.1.1.1
- look at ARP table, to get MAC address of 10.1.1.1
- not found
- send ARP request to FF:FF:FF:FF:FF:FF for 10.1.1.1
    - sent to every machine on local network
- router A responds with its MAC Address 00:11:22:33:44:55
- computer 1 saves in its ARP table
- OS assings an ephemeral PORT:50000 for the browser
- wrap TCP Datagram: with SYN flag, SourcePort=50000 and DestPort=80
- wrap IP Datagram: with Source IP and Dest IP
- wrap Ethernet Datagram built source and dest MAC address and IP Datagram as payload
- send Ethernet Datagram through network interface
    - sends data to switch

2) Switch
- receive Ethernet Frame
- extract Dest Mac
- knows the link to Router A
- forward the frame to router A

3) Router A
- receive Ethernet Frame
- unwrap Ethernet Frame (+ cheksum)
- inspect Dest MAC
- inspect TCP Dest IP
- knows to route via router B
- decrements TTL and compute new Checksum
- wrap new TCP, IP and Ethernet Datagrams updated with Dest as router B MAC and IP

4) Router B
- receive Ethernet Frame
- dissect it
- find Dest IP as computer 2
- build new ethernet frame for computer 2

5) Computer 2
- receive Ethernet Frame
- dissect message
- find it is the specified destination
- handle payload
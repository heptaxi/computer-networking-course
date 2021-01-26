# Network, Addressing and Routing

## IPv4 Address

- 32 bits long, made of 4 octets (4x8 bits) in dotted-decimal notation. 8 bits can represent numbers from 0 to 255.
- Ex: 172.16.254.1 (10101100.00010000.11111110.00000001)
- 32 bits represents numbers up to 4,294,967,295.

Important:
MAC Address is attached to a device. IP Address is attached to a network, same device can have different addresses on different networks.

Two-types:
- Static IP Address: mainly reserved to servers and network devices
- Dynamic IP Address: mainly for clients.
(not always true).

## IP Datagram (IP data packet)

IP Header composition:
- Version (4 bits): mainly ipv4 but ipv6 getting adopted
- Header Length (4 bits): almost always 20 bytes
- Service Type (8 bits): QoS, used to set priority
- Total Length Field (16 bits)
- Identification
    - used in case of a payload that needs to be split in multiple packets.
- Flags
    - indicate if a Datagram is allowed to be fragmented or has already been fragmented
- Fragment Offset
- TTL (8 bits)
    - indicate how many router hops a datagram can traverse before it's thrown away
- Protocol (8 bits)
    -   specifies which Transport Layer Protocol is used
    - mainly TCP or UDP
- Header Checksum (16 bits)
- Source IP Address (32 bits)
- Dest IP Address (32 bits)
- Options
- Padding
    - a series of 0 ensuring the header is of correct total size, complementing the Options field.

Max Size of an IP Datagram is 65,535 (16 bits).

Example usage:
Ethernet Frame (2=Datalink)
    -> IP Datagram (3=Network Layer)
        -> IP Header
        -> TCP Segment (4=Transport Layer)
            -> TCP header
            -> TCP Message
                -> Message (5=Application Layer)

## IP Address Class
- Class A:
    - 1 octet for network ID, 3 octets for host ID
    - range 0-126
    - max 16M hosts
    - first bit=0
- Class B:
    - 2 octet for network ID, 2 octets for host ID
    - range 128-191
    - max 64K hosts
    - first bits=10
- Class C:
    - 3 octet for network ID, 1 octets for host ID
    - range 192-224
    - max 254 hosts
    - first bits=110
- Class D:
    - range 224-239
    - used for multicasting
    - first bits=1110
- Class E:
    - 3 octet for network ID, 1 octets for host ID
    - range 240-255

## Address Resolution Protocol (ARP)
a protocol used to discover hardware address of a node with a certain IP address.

- ARP table: mapping of IP/MAC
- device sends broadcast ARP message to the switch, asking for the IP of a specific MAC Address, and it will receive the IP of the matching host declaring itself.

## Subnetting
splitting a largee network into smaller individual networks.
Incorrect subnetting problems are a common problem for IT Support.
It is very useful because it would be very complicated for a single router to handle traffic to all hosts in the network (16M in case of a Class A IP). Subnetting makes it easy for a router to delegate to other routers.

Switches send Datagrams to the appropriate routers based on the network ID.

Subnet Mask (32 bits)
- 4 octets in decimal
- made of 2 sequences
    - one with all 1 (network ID)
    - one with all 0 (host ID)

Ex: 
IP 
9.100.100.100 
0000 1001 . 01100 0100 . 0110 0100 . 0110 0100

Subnet Mask
255.255.255.0
1111 1111 . 1111 1111 . 1111 1111 . 0000 0000

A single 8-bit number can represent a number from 0 to 255, which is 256 hosts. But 0 is generally not used and 255 is used as the broadcast address for the subnet -> 253 hosts.

Subnet Mask
255.255.255.224
11111111.11111111.11111111.11100000
27 ones and 5 zeros

9.100.100.100/27

Network 9.0.0.0/8 is IBM.

## CIDR
IP 9.100.100.100
Mask 255.255.255.0
-> 9.100.100.100/24

--> network ID= 9.100.100
--> host ID= 100

/24 network is 8 host bits -> 2^8=256
256 -2 = 254 host IDs
254+254=508 host IDs on 2 /24 subnets

/23 network is 9 host bits -> 2^9=512
512-2=510 host IDs

## Routing
A router is a device that has at least 2 different network interfaces (since it needs to be connected to at least 2 networks) and handles how to route packets it receives, to the appropriate destination.

Basic Routing:
- Receive data packet
- Examine dest IP
- Look up IP dest network in routing table
- Forward traffic to destination

Ex:
Router A connects networks A and B:
- network A: 192.168.1.0/24
    - router A IP: 192.168.1.1
    - computer A IP: 192.168.1.100
- network B: 10.0.0.0/24
    - router A IP: 10.0.0.254
    - computer B IP: 10.0.0.10

Since Computer A knows it can not reach 10.0.0.10 through its local network, it sends the packet to it's Gateway (router A).
Since the router A is directly connected to each computer it has their MAC address in the ARP table. Updates the TTL and sets its own MAC as the source, and replaces the dest MAC address.
If router is not connected to the host, it will send it to the router.

Router B connects networks B and C:
- network A and B stay the same
- network C: 172.16.1.0/23
    - router B IP: 
    - computer C IP: 172.16.1.100

Computer A -> Router A -> Router B -> Computer C

## Routing Tables
Most Basic Routing tables have 4 tables:

- Destination:
    - this is a remote network
    - NetID and NetMask
    - Ex: CIDR 192.168.1.1/24
- Next Hop
    - a router to direct packets to a remote network
    - can have a catch all entry for networks it it directly connected to
- Total Hops
    - keep track of how "far" is a destination
- Interface
    - defines which of its interfaces to use for each network

## Interior Gateway Protocol
used for communication in a corp or ISP.
2 main protocols:
- Distance Vector Protocol (older)
    - a router sends how many hops it is from networks it knows of, to routers it is directly connected to
    - use it to update shortest path
- Link State Protocol
    - each router advertises the state of its link to every router it is connected to
    - use link state to choose more reliable path

## Exterior Gateway Protocol
Core Internet Routers.
Internet Assigned Numbers Authority (IANA) manages subjects such as IP distribution among ISPs etc.

IANA also assigns Autonomous System Number (ASN). ASN = 32 bits.

## Non-Routable Address space
IP as 32 bits can represent max 4,294,967,295 values.
Not every host on a network needs to be routable through public internet, therefor do not need to waste this 32-bit space.

Network Address Translation (NAT) handles translating a Public IP to a Private IP.

Non-Routable IP Ranges:
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

Free to use by anybody for their internal network.
# Datalink

## History
1983: Ethernet standard first publication
no switch at this time.
Collision Domain = configuration when only one device can speak at a point in time.

CSMA/CCD:
used to determine when the communication channels are clear, and when a device is free to transmit data.

## MAC Address
MAC Address = Media Access Control Address.
a globally unique identifier attached to an individual network interface. It is a 48-bit number normally represented as 6 groups of 2 Hexadecimal  numbers (=6x8 bits).

- Binary number: representation using only 2 digits (0/1)

- Hexadecimal number: representation using 16 digits (0 to 9 + A to F)

- Octet: a number that can be represented as 8 bits.

The MAC Address is composed of 2 sections of 3 octets:
- Organizationally Unique Identifier (OUI):
The first 3 octets of MAC a Address. Assigned by the IEEE.
- Vendor Assigned (NIC Cards, Interfaces, ...)

 | OUI | Vendor Assigned
Size (bits) |  24 | 24
Size (hex digits) | 6 | 6
Example | 00 60 2F | 3A 07 BC
Structure | Brand | Particular Device

## Ethernet
It uses MAC Addresses to ensurre that the data it sends has an address for both the sender and recipient of the transmission. There are multiple transmission mecanisms.

- Unicast Transmission: 
always meant for just one receiving/dest address.
At the ethernet level: if the least significant bit in the first octet of the dest MAC address is 0, ethernet frame is intended for only dest address.

- Multicast Transmission:
send the packet to every device on the LAN and let the device filter if they want to receive it.
At the ethernet level: if the least significant bit in the first octet of the dest MAC address is 1, it is a multicast transmission.

- Broadcast Transmission:
send the packet to every device on the LAN.
At the ethernet level: if the dest address is the broadcast address (FF:FF:FF:FF:FF:FF). It is mainly used for learning about the devices on the network.

## Data packet
an all-encompassing term that represents any single set of binary data being sent accross a network link.

## Ethernet Frame
an ethernet data packet.
- Preamble (8 bytes = 64 bits)
    - 7 random bytes (can be used for synchronizing clocks)
    - 1 byte SFD (Start Frame Delimiter)
- Dest Address (6 bytes = 48 bits)
- Src Address (6 bytes = 48 bits)
- VLAN Tag (4 bytes = 32 bits)
    - VLAN header indicates that the frame itself is a VLAN Frame
    - if a VLAN header is present, then Ether-Type follows it.
- Ether-Type (2 bytes = 16 bits)
    - used to describe the protocol of the contents of the frame
- Payload (0-1,500 bytes)
    - data for layers 3 to 5.
- FCS (4 bytes = 32 bits)
    - Frame CheckSum using Cyclical Redundacy Check (CRC)

## VLAN (Virtual LAN)
a technique that lets you have logical LANs operating on the same physical equipment.

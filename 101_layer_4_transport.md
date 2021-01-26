# Transport Layer
Layers 1 to 3 allow computers to communicate with each other.

Layer 4 allows traffic to be directed to specific applications.

Layer 5 allows applications to communicate in a way they understand.
Ex: client/server.

## Multiplexing

- Multiplexing: nodes on a network have the ability to direct traffic toward many receiving services.
- Demultiplexing: the node routes to the appropriate service/application.

on layer 4 (Transport) multiplexing is handle by using dedicated ports.
Ex:
- :80 for HTTP server
- :21 for FTP

## TCP
TCP Segment: TCP header + data section
- TCP Header:
    - destination PORT (16 bits)
    - Source PORT (16 bits): ephemeral
    - Sequence Number (32 bits)
        - used in case of splitting a messages in multiple frames
    - ACK number (32 bits)
        - expected Seq Number of next Segment.
    - Header Length (4 bits)
    - empty (6 bits) (offset)
    - TCP Control Flags (6 bits)
    - TCP Window (16 bits)
        - specifies the range of sequence numbers that might be sent before an ACK is required
    - Checksum (16 bits)
    - Urgent Pointer field (16 bits)
        - used in conjunction with a specific TCP Flag
    - Options (0 of 16 bits)
    - Padding (zeroes to complement Options to get to 32 bits)
        - ensure payload begins at expected position
    - Data payload (size varies)

TCP Control Flags:
- URG (urgent): segment is considered urgent and Urgent Pointer Field has details about it 
- ACK (acknowledged): current message is acked, check it
- PSH (push): transmitting devide wants receiving devince to push buffered data to app
- RST (reset): one of the side in a TCP Connection hasnt been ablee to recover from a series of missing/malformed segments
- SYN (sync): used when establishing a connection
- FIN (finish): client wants to close the connection.

### Establishing TCP Connection
The 3-way handshake.

- sender SYN
- receiver SYN/ACK with seq number
- sender ACK

Connection established, start the conversation.

### Closing TCP Connection
The 4-way handshake.

- sender FIN
- receiver ACK
- receiver FIN
- sender ACK

### TCP Sockets
Socket states:
- LISTEN: a TCP socket is listening (server side only).
- SYN_SENT: a SYNC req has been sent but connection not yet established (client side only)
- SYN_RECEIVED: a socket previously in LISTEN statee has received a SYNC req and sent SYNC/ACK
- ESTABLISHED: TCP connection established
- FIN_WAIT: FIN sent but not receivd FIN_ACK yet
- CLOSE_WAIT: connection closed at TCP layer, but app that opened the socket hasn't release its hold on the socket yet
- CLOSED: connection fully terminated, no further communication possible

## Connection Oriented or Connection-Less Protocol
- Connection Oriented ensures the transmission is reliable and devices ACK the reception of frames.
    - If one packet is mutated or failed to be transmitted, it is retried. Router can for example drop packets in case of congestion, or a cable could be cut accidently.
    - IP Layer does not retry a frame with invalid CRC, just drop them.
    - Order does not really matter, thanks to sequence numbers
- Connection-Less oriented (Ex:UDP)
    - does not rely on ACK
    - not so reliable
    - used a lot for audio/video streaming
    - does not really matter if a few packets a dropped
    - much faster because of less overhead releated to the connection

## Firewalls
a device that blocks traffic that meets certain criteria.
Ex: block IP ranges or ports.
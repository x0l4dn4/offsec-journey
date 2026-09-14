## Layer 3

### IPv4

The IPv4 **TTL** field is 8 bits in length and specifies the maximum number of router hops a packet may traverse before it must be discarded. Each router that forwards the packet is expected to decrement the TTL value by 1.

If a router decrements the TTL to zero, it discards the packet and sends an `ICMP TTL Exceeded in Transit` message—ICMP Type 11, Code 0—to the source IP address of the discarded packet.

The source address of this ICMP message is the router that discarded the packet. This behavior makes it possible to trace a network path by identifying the intermediate router hops between the scanning system and the destination.

### IPv6

IPv6 does not use a field named `TTL`. Instead, it uses a **Hop Limit** field. The different name avoids suggesting that the value represents time, although its behavior is similar: every router decrements the field by 1 as it forwards the packet.

Once the Hop Limit reaches zero, the packet is discarded. This mechanism can therefore be used to identify the sequence of router hops between a source and a destination.

## Layer 4

### TCP

Legitimate TCP connections begin with a three-way handshake. During this process, the endpoints exchange sequence numbers and control flags, which allow them to track packet delivery, retransmit lost packets, and place received packets in the correct order. The TCP specification refers to these flags as **control bits**.

> Every legitimate TCP connection begins with a TCP three-way handshake. The handshake exchanges sequence numbers so that lost packets can be retransmitted and packets can be placed in the correct order.

TCP control bits are located in the TCP header. They indicate the state of a connection and identify how a particular packet relates to that connection.

TCP has six traditional control bits and two additional bits defined by RFC 3168:

- **SYN**: Requests the synchronization of sequence numbers. This bit is used when establishing a connection.
- **ACK**: Indicates that the acknowledgment field is valid and that the packet acknowledges previously received data.
- **RST**: Requests that the connection be reset because of an error or another interruption.
- **FIN**: Indicates that the sender has no more data to transmit and that the connection should be closed gracefully.
- **PSH**: Requests that buffered data be delivered to the receiving application without unnecessary delay.
- **URG**: Indicates that the urgent pointer is valid and that the packet contains urgent data.
- **CWR**: Congestion Window Reduced. Indicates that the sender has reduced its congestion window in response to network congestion.
- **ECE**: Explicit Congestion Notification Echo. Indicates that congestion was detected on the connection.

Each control bit can be set independently of the others. As a result, a packet can have multiple control bits set at the same time—for example, a packet can be both `SYN` and `ACK`. Each bit has a value of either `0` or `1`.

According to the original TCP specification, RFC 793, when a service is listening on a TCP port and receives a packet with the `SYN` control bit set, the TCP implementation must respond with a `SYN-ACK` packet.

> This response must be sent regardless of the payload carried by the `SYN` packet.

Consequently, it is possible to test whether a TCP port is open without knowing which service is listening on it. Sending a `SYN` packet and observing the response provides a reliable way to determine whether the port is open or closed.

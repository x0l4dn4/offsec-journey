
## IPv4 

The TTL field is 8 bits long and indicates *how many hops* this packet can travel before it must be discarded.
When a router receives a packet, it is *supposed to decrement the TTL field by 1*.

When a given router decrements the TTL to zero, the router is supposed to drop the packet and send a `TTL Exceeded in Transit` message (ICMP Type 11, Code 0) back to the source IP address of the discarded packet.

The source address of this `ICMP TTL Exceeded in Transit` message is the router itself. This interesting TTL behavior allows us to perform network tracing, *discerning the hops* between the scanning machine and target systems.


## IPv6

There is no `TTL` field, but `Hop Limit` named like that to remove any connotation of time from it, but it is still decremented by each router hop as the packet moves from its source to its destination.

Therefore, it can be used it to determine the series of router hops between a source and destination.

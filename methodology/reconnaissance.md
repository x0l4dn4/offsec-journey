> Note this is just a draft. Notes has not been completed.

The goal of reconnosinance or scanning phase is to learn more about the target environment by interacting with it.
Some objectives are determining the assigned IP addresses of clients, servers, firewalls, routers, layer-three switches and other networked devices.

Also it is important to learn the topology of the target environment creating a diagram or a *network map* that shows how the hosts and network devices interconnect.

Additionally, determining the operative system of target devices so in the explotation phase proper vulnerabilites based on the OS can be used.

After that, listing open TCP and UDP ports that can be compromised and verify which service is listening on each  port and the version of the given application that can have a potential vunerability based on the version.

> Network sweep -> Port scan -> OS fingerprint -> Version scan -> Vulnerability scan


**Network sweep**: Identify live hosts and IP addresses via active probing. If any type of response is received from the target, including a RST, there is likely a live system at that address.

**Port scan**: Find open TCP and UDP ports by probing the available targets (often discovered via the network
sweep). If an open port is found, it means a service is both listening and accessible from the current location.

**OS fingerprinting**: Different operative systems respond differntly to specific probes. Use these
probes and their often-unique responses in an attempt to determine the target operating system.
Sending these packets to the targets is called *active OS fingerprinting*.
Alternatively, some sniffing tools include functionality to discern what type of operating system formulated given packets in an entirely passive sense.

**Version scan**: The attacker needs to know the service, and ideally the software version, running on the remote
port. Although many major services listen on well-known ports (sshd on TCP 22 HTTP on TCP 80), a sysadmin may put these services on alternative ports.
By interacting with ports during a version scan, protocols they speak and possibly the version of the remote service can be checked.

**Vulnerability scanning**: In these scans the target machine it is measure wheter it  has any one of thousands of
potential vulnerabilities, which could include misconfigurations or unpatched services.


Nmap means network mapper

### Passive reconnosinance


Scanners also are good at determining firewall rules and other access control policies.

A sysadmin can verify his firewall is working properly using these techniques.

Similarly, an attacker can use the same tricks to find holes in firewall coverage or simply learn the firewall rules to tailor his attack.

Most Internet applications communicate using either the `TCP` or `UDP` protocols.

Both protocols use the concept of ports to allow for *multiple applications to coexist on a single IP address*.

Because the *port fields* in both the TCP and UDP *headers* are allocated exactly 16 bits of data, the total number of unique port numbers that can be represented is 2 raised to the power of 16.
Both UDP and TCP support 65,536 (2^16) distinct ports that applications can choose to bind to.


> The file `/etc/services` on most Unix machines contains a mapping of common applications to their default port number. 


> Some applications such as `PortSentry` exist for the sole purpose of confusing or frustrating port scans. Additionally, firewall features like `SYN-cookies` can make *ports appear open* when they are actually closed.


The first step in a vulnerability assessment in network discovery. This reconnaissance stage determines what IP address ranges the target is using, what hosts are available, what services those hosts are offering, general network topology details, and what firewall/filtering policies are in effect.

You must determine the IP ranges. Normally the company will explicity specified what networks they want to be test and decode the CIDR notation.


Start with a list scan (-sL). This option simply enumerates every IP address in the given target netblock and does a reverse-DNS lookup (unless -n was specified) on each host. The names of the hosts can hint at potencial vulnerabilites and allow for a better understanding of the target network.

The primary purpose of performing a list scan in Nmap using the -sL option is to simply *list the targets* you specify without sending any packets to the target hosts.

So it verify the exact list of IP addresses that Nmap would scan before you actually launch an active, potentially disruptive scan. This ensures you do not accidentally scan unauthorized or sensitive systems.

> Because the list scan does not check if the target computers are actively up and running, it will report "0 hosts up" at the end of the scan.

This makes it an excellent tool for stealthy, passive reconnaissance where you want to gather hostname information from DNS servers without alerting the target systems themselves.


Nmap features that try to determine the application and version number of each service listening on the network.

Also requests that Nmap try to guess the remote operating system via a series of low-level TCP/IP probes known as OS fingerprinting. This sort of scan is not at all stealthy.


> In a pentesting situation, you often want to scan every host even if they do not seem to be up. After all, they could just be heavily filtered in such a way that the probes you selected are ignored but some other obscure port may be available.



Nmap default, which is `-PE -PS443 -PA80 -PP`
These are all host discovery techniques (ping types) used in combination to determine which targets on a network are really available and avoid wasting a lot of time scanning IP addresses that are not in use. TODO READ ABOUT IT



### Types of scans.

#### TCP scan

The `SYN` scan relies on three-way handshake process in which the initial `SYN` packet is sent to a target host, which will respond with a `SYN-ACK` packet if the port is open.

However, the *port scanner does not complete the TCP three-stage handshake* by sending an `ACK` packet but instead sends a `RST` packet, shutting down the connection.

This is referred to as a `stealth scan`, as Unix systems would record or log a connection attempt only if the three-stage handshake were completed.

> Using the SYN scan, the scanner could determine which ports were open on a remote system without being logged.


If no service is listening on a scanned port, the attacker will not receive a `SYN/ACK`. Depending on the configuration of the target's operating system, the attacker could receive an `RST` packet in return, indicating that the port is *closed*.

Alternatively, the attacker may receive no response at all. No response could mean that the port is *filtered* by an intermediate device, such as a firewall or the host itself or there is no host up at that IP address.

On the other hand, it could just be that the response was lost in transit. Thus, while this result typically indicates that the port is closed, but you don't know.

#### UDP scan

UDP scanning is a bit more difficult that TCP scanning. Unlike TCP, UDP does not use handshakes. 
So the very first packet sent goes directly to the application. UDP applications are prone to discarding packets that they can't parse, so scanner packets are likely to never see a response if an application is listening on a given port.

> However, if an UDP packet is sent to a port without an application bound to it, the IP stack returns an **ICMP port unreachable** packet.

The scanner can assume that any port that returned an *ICMP error* is closed, while ports that didn’t return an answer are either open or filtered by a firewall. It is hard to distinguish between open and filtered ports in UDP.



#### Host discovery

The -s* options select *scan types*

##### List Scan (-sL)

This merely lists hosts for scanning, including reverse DNS lookups. It is often surprising how much useful information simple hostnames give out. However, **no traffic is sent to the targets**. This is useful for validating the range of IPs you are working with. If a host's domain name is not recognizeable, it is worth investigating further to prevent scanning the wrong company's network.


##### No port Scan (-sn)

Ping Scan allows you to effectively run a ping sweep against the targets that is, there is no port scanning, but unlike the List Scan, we are sending data in the form of pings (specifically, an ICMP ECHO request) to the targets. 

The default host discovery done with `-sn` consists of an ICMP echo request, TCP SYN to port 443, TCP ACK to port 80, and an ICMP timestamp request by default.

When executed by an unprivileged user, only SYN packets are sent (using a connect call) to ports 80 and 443 on the target.

When a privileged user tries to scan targets on a local ethernet network, ARP requests are used unless `--send-ip` was specified. If the IP addresses being scanned are on the same subnet as the scanner, ARP packets are used instead; it is a faster and more reliable way to see which IP addresses are in use. 

If the subnet scanned is local, Nmap is nice enough to look up the MAC addresses in its database to tell you who manufactured the network card.

If ICMP packets are blocked, you can also use TCP ACK packets. This is often referred to as a *TCP Ping*.

The RFC states that *unsolicited ACK packets should return a TCP RST*.

So, if you send this type of packet to a port that is allowed through a firewall, such as port 80, the target should respond with an RST indicating that the target is active.

Scanning UDP is more difficult as it is a *connectionless protocol* and does not use a handshake like TCP. With UDP, the following sequence is used:
- Source sends UDP packet to target
- Target checks to see if the port/protocol is active then takes action accordingly

This makes scanning UDP ports especially challenging. If you receive a response, it will be one of three types: an ICMP type 3 message if the port is closed and the firewall allows the traffic, a disallowed message from the firewall, or a response from the service itself. Otherwise, no response could mean that the port is open, but it could also mean that the traffic was blocked or simply didn't make it to the target.

Many administrators tend to focus more on securing TCP-based services and often don't consider UDP-based services when determining their security policies. With this in mind, you can sometimes find (and exploit) vulnerabilities in UDP-based services, giving you another potential entry point to your target system.

###### Ping sweeps


The -P* options select *ping types* or *probe types*.

##### -Pn (No ping)

Sending ping (ICMP echo request) packets used to be a reliable way to determine whether a computer was listening at a given IP address.
 
These days, with firewalls becoming more widely deployed, ping packets are sometimes blocked by default.

You can use `-Pn` option which instructs nmap to bypass the host discovery process entirely and instead connect to every port even if the host seems down.

Disabling host discovery with `-Pn` causes Nmap to attempt the requested scanning functions against every target IP address specified. So if a /16 sized network is specified on the command line, all 65,536 IP addresses are scanned as if each target IP is active. Default timing parameters are used, which may result in slower scans.

For machines on a local ethernet network, ARP scanning will still be performed (unless --disable-arp-ping or --send-ip is specified) because Nmap needs MAC addresses to further scan target hosts.

A faster solution to the blocked ping problem is to extend the list of probed ports (using -P* options) to cover more than just pings and TCP port 80 and 443.




###### See also:

https://nmap.org/book/man-host-discovery.html



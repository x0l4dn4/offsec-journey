> Note this is just a draft. Notes has not been completed.

### Passive reconnosinance


Scanners also are good at determining firewall rules and other access control policies.

A sysadmin can verify his firewall is working properly using these techniques.

Similarly, an attacker can use the same tricks to find holes in firewall coverage or simply learn the firewall rules to tailor his attack.

Most Internet applications communicate using either the `TCP` or `UDP` protocols.

Both protocols use the concept of ports to allow for *multiple applications to coexist on a single IP address*.

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

##### DNS and subdomains passively

robots.txt check it out if a web server is available on the exam.

XML sitemap  author-sitemap.xml,  category-sitemap.xml page-sitemap.xml

```bash
host www.danielcallejo.dev
```


```bash
dnsrecon -d www.danielcallejo.dev
```


https://dnsdumpster.com/

```bash
whois
```

https://who.is/



```bash
sublist3r -d hackersploit.org 
```


##### Website footprinting passively

Builtwith, and wappalizer extension

```bash
whatweb https://danielcallejo.dev/
```


https://sitereport.netcraft.com/  Fow downloading entire websites


https://github.com/enablesecurity/wafw00f  Firefall footprinting tool


Google dorks  [Google Hacking Database (GHDB) - Google Dorks, OSINT, Recon](https://www.exploit-db.com/google-hacking-database)


Use waybackmachine to find sensitive information that could have been removed


## Enumerating emails passively

theHarvester also gets subdomains, IPs...

```bash
theHarvester -d ine.com -b duckduckgo,yahoo,baidu
```




haveIbeenPwned when obtaining emails for targets



## Actively Reconnissance

DNS interrogation is the process of enumerating DNS records for a specificic domain.

In some cases, DNS server admins may want to copy or transfer zone files from one DNS server to another. 

If misconfigured and left unsecured, this functionality can be abused by attackers to copy the zone file from primary DNS server to another DNS server.



dnsenum

```bash
dnsrecon -d zonetransfer.me
```

```bash
fierce --domain zonetransfer.me
```

dnsdumpster


/etc/hosts  


## Scanning with nmap

```bash
sudo nmap -sn 192.168.1.0/24  # Note the use of sudo, look why - Ping scan
```

When lunching nmap with no options you are performing a SYN scan on a thousand of the most frecuently used ports. When you are dealing with a Windows machine will typically block ICMP pings by default. (Host seems down).

We can use the -Pn option instead.

To list all the ports -p- option or -p1-1000  from 1 to 1000

-sU to perform a UDP scan.


LIstar rutas disponibles en un servidor web

gobuster


Each OS has a TTL so you can guess which OS is being detected depending on the TTL. Using nmap -O can send a lot of packages, here just with a ping we can see the TTL

It is true that the TTL can be spoofed / manipulated
but we are talking about defaults

> Note this is just a draft. Notes has not been completed.

### Passive reconnosinance




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



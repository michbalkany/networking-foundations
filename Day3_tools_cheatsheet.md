ping - Checks if your device is reachable and how long it takes to reply

tracerout /tracert - shows the path your data takes across the internet to reach its destination

ip or ifconfig - shows your device's IP address and network setup

netstat or ss - Shows open connections and what apps are using the network

dig or nslookup - Looks up DNS info - like turning a domain name into an IP address

tcpdump - sniffs all the network traffic your device can see - like a packet level spy cam

whois - Finds out who registered a domain or IP - sort of like peeking at their internet business card.


Tool		Notes
ping	    Use ping -c 4 to limit count
traceroute	May need to install with sudo apt install traceroute
ip		    ip addr show is modern replacement
ifconfig	Run sudo apt install net-tools if missing
netstat	    Try ss -tuln instead—it's modern
ss	        Preferred over netstat on modern distros
dig		    If not installed: sudo apt install dnsutils
nslookup	Part of dnsutils
tcpdump		Run with sudo
whois		If missing: sudo apt install whois


## tcpdump: Real Packet Capture Summary
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
## ---> The Two lines below are telling me that my VM is using NTP (Network Time Protocol). it's talking to an external time server (time2.tritan-bb.net) using UDP port 123
## ^^^^^^^^^^^^^^^^^^^^^
17:11:22.339995 IP 192.168.113.128.56159 > time2.tritan-bb.net.ntp: NTPv4, Client, length 48
## DNS Reverse lookup
17:11:22.410632 IP 192.168.113.128.59411 > 192.168.113.2.domain: 60347+ PTR? 128.113.168.192.in-addr.arpa. (46)

 ## The line above ^^^^^ is my VM asking the local DNS server @ 192.168.113.2 "Who owns the IP 192.168.113.128" PTR records are used for reverse DNS lookups, most of the time by, security software to attach domain names to IPs

17:11:22.417457 ARP, Request who-has 192.168.113.128 tell 192.168.113.2, length 46
17:11:22.417514 ARP, Reply 192.168.113.128 is-at 00:0c:29:f5:16:04 (oui Unknown), length 28

## The two lines above are ARP Requests and replies. Device 192.168.113.2 is trying to find the MAC address of my VM. My VM then replies with its virtual MAC: 00:0c:29:f5:16:04

17:11:22.418359 IP time2.tritan-bb.net.ntp > 192.168.113.128.56159: NTPv4, Server, length 48
17:11:22.470319 IP 192.168.113.2.domain > 192.168.113.128.59411: 60347 NXDomain*- 0/0/0 (46)
17:11:22.471023 IP 192.168.113.128.37075 > 192.168.113.2.domain: 44087+ PTR? 8.248.142.23.in-addr.arpa. (43)
17:11:22.555713 IP 192.168.113.2.domain > 192.168.113.128.37075: 44087 1/0/0 PTR time2.tritan-bb.net. (76)
17:11:22.557453 IP 192.168.113.128.35956 > 192.168.113.2.domain: 12454+ PTR? 2.113.168.192.in-addr.arpa. (44)
17:11:22.602498 IP 192.168.113.2.domain > 192.168.113.128.35956: 12454 NXDomain*- 0/0/0 (44)

# mDNS (multicas DNS) -- Local device Discovery
17:11:25.203866 IP 192.168.113.1.mdns > mdns.mcast.net.mdns: 0 PTR (QM)? _spotify-connect._tcp.local. (45)
# whats happening here is that my host machinge or router is broadcasting local services over the network such as spotify via multicast.



# Using Nmap:

sudo nmap -sn 192.168.113.128/24
[sudo] password for mich: 

Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-13 17:58 EDT
Nmap scan report for 192.168.113.1
Host is up (0.00045s latency).
MAC Address: A6:CF:99:D5:FE:65 (Unknown)
Nmap scan report for 192.168.113.2
Host is up (0.00086s latency).
MAC Address: 00:50:56:F1:09:C4 (VMware)
Nmap scan report for 192.168.113.254
Host is up (0.00068s latency).
MAC Address: 00:50:56:F2:77:C4 (VMware)
Nmap scan report for 192.168.113.128
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 2.63 seconds

# What these results show, is the entire local subnet (192.168.113 -192.168.113.255) to see which devices are currently online AKA a ping sweep.

# I discovered 4 active devices on the local network:
192.168.113.1 -- this is the virtual router or gateway. it handles traffic n/out of my VM

192.168.113.2 -- A VMware virtual service or my host. MAC shows its part of VMware.

192.168.113.254 -- Another VMware virtual network device which could be DHCP or NAT service

192.168.113.128 -- this is my Kali linux VM itself, the actual machine that is running the scan.

# How would a white hat hacker be able to use these tools on a day to day job?

# Ping - Checks if the target is reachable before scanning or exploiting it.

# traceroute - Map the path to a target server, find network chokepoints or ISP leaks.

# ip/ifconfig - checks IP and interface details before launching an attack or scan from the right interface.

# netstat/ss - See what service are running on a system during post-exploitation 

# nslookup - Gathers DNS intel such as subdomains, mail servers, or internal naming conventions AKA footpringting

# tcpdump - Captures credentials, cookies, or sensitive data from unencrypted traffic duirng sniffing.

# nmap -sn -- discovers live hosts to scan further- stage 1 of a network scan

# arp -a -- identifies all devices on a LAN and spot possible spoofing or rogue entries.

During a penetration test, you would use-- 
nmap -sn to locate targets --> then verify their presence with ping --> use tcpdump to sniff open wifi-traffic --> leverage nslookup/dig to find admin panel subdomains --> use netstat to find services like SSH MySQL to exploit.
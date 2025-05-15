# Objective:
- Identify devices on my LAN
- Document IPs, MACs, and hostnames
- Build a simple topology map
- Practice recon that real red/blue teams do every day.


1) # Scan my network for all devices:
- Sudo nmap -sn 192.168.113/24

IP Address      ||  MAC Address             ||  Hostname / notes
192..168.113.1    A6:CF:99:D5:FE:65             (Unknown) Probably a virtual router
192.168.113.2     00:50:56:F1:09:C4             (VMware) VMware virtual network interface
192.168.113.254   00:50:56:F2:77:C4             (VMware)VMware virtual network interface
192.168.113.128   (self)                        My Kali Linux VM (no MAC shown)

2) # Use ARP table for extra intel:
- ip neigh show
# Results
IP Address  ||	    Interface   ||	MAC Address     ||	State   ||	Notes
192.168.113.2       eth0          00:50:56:f1:09:c4    STALE        Matches nmap results
172.17.0.0          docker0       none                 FAILED       Docker bridge, not a host
192.168.113.254     eth0 lladdr   00:50:56:f2:77:c4    STALE        matches nmap host


3) # Build a Topology Diagrm:

4) # Write a recon summary:

- after scanning the subnet 192.168.113.0/24 using ` nmap -sn ` i foudn 4 live hosts/devices.
- 192.168.113.1 is likely a virtual router or gateway, the MAC address is not recognized (could be custom or unknowned vendor)
- 192.168.11.2 is a VMware virtual interface. Consisten with host system ir virtual bridge.
- 192.168.113.254 is another VMware MAC, possibly DHCP or NAT service.
- 192.168.113.128 is my own Kali VM
- ARP table output confirmed MAC addresses for `.2` and `.254`, consistent with Nmap results.
- there were no unknown rogue hosts devices discovered.all network behavior seems consistent with a controlled virtual lab environment. 


## Recon Summary

Using `nmap -sn` to scan the `192.168.113.0/24` subnet, I identified four active devices on the local network. These included the default gateway at `192.168.113.1` (with an unknown MAC vendor), two VMware-associated interfaces at `192.168.113.2` and `192.168.113.254`, and my own Kali Linux VM at `192.168.113.128`. All discovered MAC addresses matched the expected VMware OUI pattern, except for the gateway, which could not be resolved to a known vendor. Cross-referencing the results with `ip neigh show`, I confirmed that `.2` and `.254` had matching MAC addresses in the ARP cache, supporting the Nmap data. No unexpected hosts or rogue devices were identified. The network topology and device roles are consistent with a VMware-managed NAT environment. If this were an active recon engagement, the next steps would include deeper scans (e.g., `nmap -sV -O`) to enumerate services and detect operating systems for each live host.


5) # Common Mistakes

# - Scanning the wrong network
Mistake: Scanning 192.168.1.0/24 when youre actually on 192.168.113.254/24
Why it happens: New analysts assume a default subnet without checking their IP.
How to avoid: Always run ip a or ipconfig before any scan.

# - Ignoring ARP output
Mistake: Not cross referencing ip neigh show with you Nmap results.
Why it matters: ARP can reveal devices that may not respond to ICMP/Nmap- especially stealthy ones.

# - Not noticing MAC vendors
Mistake: Overlooking MAC vendor prefixes like 00:50:56 = VMware

Why it matters: Recognizing VMware, Cisco, Apple, etc can help you identify devide types- important for red or blue teams

# - Assuming every host is legit
Mistake: Seeing only 4 hosts and assuming that means the network is clean.

Why it's dangerous: Some rogue devices may have firewalls, ICMP disabled, or intentionally spoof MACs/IPs to avoid detection

What are the Subnets??
Subnets are the process of dividing IP networks into smaller networks for better management AKA sub-networks or subnets for short. it defines how many devices can be on each subnet and helps organize IP address allocation efficiently.

Why do we use Subnets? 
- improves network performance by reducing broadcast traffic
- enhances security by isolating different segments
- optimizes IP usage by prreventing waste
- simplifies managment of large networks

What does CIDR notation mean?
Classles inter domain routing defined how many bits are used for the network portion of an IP address.

more bit in CIDR = few hosts(small sub network)
less bits in CIDR  = more hosts(larger sub network)

What is the difference between Public vs Private IP Ranges?
Private IP ranges are reserved for internal use and not routable on the public internet.

Public IPs are all other addresses that are routable over the internet and need to be assigned uniquely.




Step 1: Understanding and calculating subnets:

Each Device needs a network. that would be requiring an IP address (192.128.1.0) AND a subnet mask (255.255.255.0) to better understand what network its on.



Step 2 IP address structure:

each IP address has 4 parts to it, ranging from 0-255. these are called octets.

if im looking at an IP address such as 192.168.1.10/24 -- 19.128.1.10 is the IP and /24 is the CIDR notation which ultimately means that:

/24 bits for network and 8 bits for the host since there are 32 bits in total.

What Does The Subnet Mask Mean??

CIDR	Subnet Mask	    Host Bits	    Usable Hosts
/24	    255.255.255.0	    8	        2⁸ − 2 = 254
/25	    255.255.255.128	    7	        2⁷ − 2 = 126
/26	    255.255.255.192	    6	        2⁶ − 2 = 62
/27 	255.255.255.224	    5	        2⁵ − 2 = 30

We will always subtract 2:
1 IP for the network address 
1 IP for the broadcast address


Step 3: Calculate Subnets:

if I am given:

IP: 192.168.1.0/26, how many subnets and how many IPs per subnet?

first we calculate how many usable hosts for the IP:

/26 = 26 bits for the network --> 32-26 = 6 --> 2 to the power of 6 - 2 = 62 --> we see that now each subnet has 62 usable IPs.

What are the Subnets?

we begin by adding 64 since 2 to the power of 6 = 64
192.168.1.0 --> 192.168.1.63
192.168.1.64 --> 192.168.1.127
192.168.1.128 --> 92.168.1.191
192.168.1.192 --> 92.168.1.255

Thus leaving us with 4 subnets it total.


Examples:

192.168.1.0/27:

Subnet mask: 255.255.255.224

Total # of hosts: 30

Network address: 192.168.1.0

First usable IP: 192.168.1.1

Last usable IP: 192.168.1.30

Broadcast address: 192.168.1.31


192.168.1.0/26:

Subnet mask: 255.255.255.192

Total # of hosts: 62

Network address: 192.168.1.0

First usable IP:192.168.1.1

Last usable IP: 192.168.1.62

Broadcast address: 192.168.1.63

192.168.1.0/25:

Subnet mask: 255.255.255.128

Total # of hosts 126

Network address 192.168.1.0

First usable IP: 192.168.1.1

Last usable IP: 192.168.1.126

Broadcast address: 192.168.1.127

192.168.1.0/24:

Subnet mask: 255.255.255.0

Total # of hosts 254

Network address 192.168.1.0

First usable IP: 192.168.1.1

Last usable IP: 192.168.1.252

Broadcast address: 192.168.1.255
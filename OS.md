xfreerdp /v:10.50.29.185 /u:student +clipboard +dynamic-resolution +glyph-cache
http://10.50.21.56:8000/  
STACK# ~3  10.50.26.137 
DYMI-23-003
DYMI-23-003
ssh -J andy.dwyer@10.50.26.137 garviel@10.3.0.6

INFO 
------------------------------------------------------------------------------------------------------------
5 min per question


Methods are things you can do to an object  
Properties are information about an object  
  
------------------------------------------------------------------------------------------------------------
### Types of Persistence
$profiles
Reg run keys 
Scheduled tasks

### Examples
  
gps | get-member | where-object {$_.Membertype -match "Method"}  
get-host | select version  
get-variable 
get-verb    
gps | select name,id,path | where {$_.ID -lt "1000"}  
  
-erroraction silently continue 

------------------------------------------------------------------------------------------------------------  
### Windows subsystems 
get-cimclass * | ft -wrap
  
------------------------------------------------------------------------------------------------------------  
### Powershell Profiles
Can be a form of persistence because profiles are read when the system is started or powershell is loaded

Test-Path -Path $profile.currentUsercurrentHost    4
Test-Path -Path $profile.currentUserAllHosts       3
Test-Path -Path $profile.AllUsersAllHosts          1
Test-Path -Path $profile.AllUserscurrentHost       2

New-Item -ItemType File -Path $profile -Force 


------------------------------------------------------------------------------------------------------------  
Get-pssession


### Registry  

regedit.exe  

    -GUI  

    -Located at "C:\Windows\regedit.exe"  

reg.exe  

    -CLI  

    -Located at "C:\Windows\System32\reg.exe"  

    -Minimum commands to know  

        -Reg add, reg query, reg delete  

#### Different tools to view/manipulate the registry  

    -Powershell  

        -Root Hive Keys loaded as powershell drives  

        -Commands used  

            -get-item, get-itemproperty, get-childitem  

            -set-itemproperty, new-item, new-itemproperty  
            

#### Mount a Remote Registry via Regedit GUI  

Open the Regedit GUI  
Click on *File* => *Connect Network Registry*  
Type *File-Server*  
Click on *Check Names* Button (File-Server will become underlined)  
Click *OK*  
 
#### Mount a Registry Hive With PSDrive

    -Create Temporary or Permanent connections to navigate the registry

        -New-PSDrive -Name HKU -PSProvider Registry -Root HKEY_USERS

#### Using the Registry

    Outcome: The "Using the Registry" section covers the practical uses of the Windows Registry for both offensive and defensive purposes. Cover that any changes are only used if the process reads the new value from the registry. Cover the registries most commonly used for persistence. Cover the registries most commonly used for forensics.

#### Registry Changes

    -Often require restart

        -Either entire system or just a program

    -Some changes take effect immediately

    -Some parts of registry are always in memory

#### Registry locations that can be utilized for persistence

    -HKLM\Software\Microsoft\Windows\CurrentVersion\Run

    -HKU\<SID>\Software\Microsoft\Windows\CurrentVersion\Run

    -HKLM\SYSTEM\CurrentControlSet\services

    -HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders

    -HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders

    -HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon


**Use "psexec -s -i regedit" from administrator cmd.exe to view the SAM**

#### Commands

-PS C:\Windows\system> Remove-Item -path Registry::HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce 
-PS C:\Windows\system> Get-ItemProperty -Path Registry::HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce 

-The majority of registry keys that are relevant to attackers and defenders are in the HKLM hive 

-SAM - security accounts manager (SAM) - local accounts and groups; restricted access %SystemRoot%\system32\config\Sam 

new-psdrive -name fileserver -psprovider filesysten -root "\\file-server\warrior share"
**find usb**  
reg query hkey_local_machine\system\currentcontrolset\enum\usbstor
### Alternate Data stream
set-content .\coffe.txt -value " olson's social security number is 123-43-2341 " -stream secret.info
cat coffee.txt -stream secret.info
get-item coffee.txt -stream *
add-content -path coffee.txt -value 'dim oshell' -stream 'secret.vbs'

**Find alternate data streams**  
gci -r | % {gi $_.FullName -stream *} | where stream -ne ':$DATA'11.   
**Read alternate stream of data**  
gc '.\The Fortune Cookie' -stream none  


### Linux
------------------------------------------------------------------------------------------------------------  

**INFO**
ssh -h andy.dwyer@10.3.0.2 garviel@10.3.0.6

#### Commands
hostname || uname -a ---name of the host
whoami                 ---shows who you are logged on as
w || who             ---show who else is logged on
ifconfig || ip addr    ---displays network interfaces and configured IP addresses
ip neigh || arp      ---displays MAC addresses of devices observed on the network
ip route || route      ---shows where packets will be routed for a particular destination address
netstat (-anop) || ss---will show network connections or listening ports
nft list tables || iptables -L ---firewall rules
sudo -l                ---displays commands user can run as su
**permissions**
{a}{b} {c} {d}
 x-xxx-xxx-xxx
a-file type
b-user/owner
c-group
d-others
4-read
2-write
1-execute


find /-perm /2000 2> /dev/null -exec ls -la {} \;


## Windows Boot Process

    BIOS:

Master Boot Record

bootmgr

BCD

OSLoader

    Winload.exe, Winresume.exe

Ntoskrnl.exe

    UEFI:

GPT

bootmgrfw.efi

BCD

OSLoader

    Winload.efi, Winresume.efi

Ntoskrnl.exe

-----------------------------------------------------------------------------------------

## See what type of system on BIOS vs UEFI

-----------------------------------------------------------------------------------------

Get-Content C:\Windows\Panther\setupact.log | select-string "Detected boot environment"

bcdedit | findstr /i winload

    .exe is BIOS .efi is UEFI

GUI: System information

    BIOSMODE = Legacy (BIOS)

    

    

-----------------------------------------------------------------------------------------

## BCDEDIT - CMD prompt

-----------------------------------------------------------------------------------------

https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/bcdedit

https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/boot-options-identifiers?source=recommendations

bcdedit /?

Make Backup

bcdedit /export C:\coffee_bcd

Restore

bcdedit /import C:\coffee_bcd

#### Edit Value

bcdedit /set {current} description "Windows 11 -Coffee Box"

bcdedit /deletevalue {current} "element"

#### Create New Partition

bcdedit /create {ntldr} /d "Windows XP Pro SP3 - Tea Sucks"

Specify Partition

bcdedit /set {ntldr} device partition=C:

Specify Path to ntldr

bcdedit /set {ntldr} path \ntldr

Specify Order to be displayed in

bcdedit /displayorder {ntldr} /addfirst

Delete partition

bcdedit /delete {ntldr} /f      (/f for force)

### Linux Boot Process

**INFO**
systemV = bombadil - Minas tirith ~ oldest box ~ 
systemD = garviel - Terra ~newest box ~ 

sysv 0,1,2,3,4,5,6
sysd 







Big Mike Got Killed In Russia 1 (SysV)  
    -BIOS -Basic Input Output System

    -MBR -Master Boot Record

    -GRUB -Grand Unified Bootloader

    -Kernel

    -Init (SysV or SystemD)

    -Runlevels
**MBR Image**
https://git.cybbh.space/os/public/raw/master/images/mbrPartitionTable.png  

lsblk  
sudo xxd -l 512 -g 1 /dev/vda  
sudo dd if/dev/vda of=MBRCopy bs=1 count=512  
**see system callls from when a command is run**  
sudo strace <command>  
sudo lsmod  
cat /etc/inittab  ###finding the default runlevel  
ls -lisa /lib/systemd/system/default.target  
ls -l /lib/systemd/system  
ls -l /etc/systemd/system  
ls -l /run/systemd/generator  
ls -l /etc/rc3.d --- run level scripts  
ls -l /etc/rc1.d --/  
cat /etc/systemd/system/display-manager.service | tail -13  

systemctl cat graphical.target  
systemctl show -p wants graphical.target  
systemctl list-unit-files  
systemctl list-dependencies graphical.target  
ls -l /sbin/init  


# Windows processes  
**Echotrail.io**
- Discovering Hidden Processes, Services, and finding Normal and Abnormal Activity   
Get-Process SMSS,CSRSS,LSASS | Sort -Property Id  
Get-Process | Select Name, Id, Description | Sort -Property Id  
Get-Process | Select Name, Id, Path  
Get-Ciminstance Win32_service | Select Name, Processid, Pathname  
Get-Process | Select Name, Priorityclass  
get-process chrome | foreach {$a} {$_.modules} | more  
get-process chrome | foreach {$a} {$_.modules} | where-object modulename -like '*chrome*' | more  
get-process -name "chrome" | select-object -expandproperty modules | more  
get-process | select name, priorityclass
-Display service information for each process without truncation    
tasklist /svc    
Tasklist /m  
tasklist /m /fi "IMAGENAME eq chrome.exe"  
tasklist /fo:{table|list|csv}`  
**services**  
-cmd  
scqueryx type=service state=inactive  
scqueryx type=service state=active  
-pwsh  
get-service | where-object {$_.status -eq "running"}  
psservice
-gui  
services.msc  
### Scheduled Tasks
-View all properties of the first scheduled task.
Get-ScheduledTask | Select * | select -First 1
-gui
taskschd.msc

### Network Connections
Get-NetTCPConnection -State Established
netstat -anob

system processes run from C:\Windows\System32
users and 3rd party run from C:\"program files"






# UAC 
-Color-coded consent prompts
Red - Application or publisher blocked by group policy
Blue & gold - Administrative application
Blue - Trusted and Authenticode signed application
Yellow - Unsigned or signed but not trusted application




# Sysinternals
**mounting sysinternals**   
net use * http://live.sysinternals.com  
./procmon.exe
Procmon 
Autoruns
Procexp
TCPView
PsExec --X
PsLoggedon
LogonSessiosn
PsList --X
PsInfo


# Linux Process Validity
    -There are two primary processes after startup:

           +For kernel-space processes [kthreadd] ( PID = 2 )

           +For user-space processes /sbin/init ( PID = 1 )

    -All kernel processes are fork()ed from [kthreadd] and all user processes are fork()ed from /sbin/init or direct ancestor.

    -Kernel processes are typically used to manage hardware, are directly handled by the kernel,have their own memory space, and have a high priority.

    -They can be identified by the name enclosed in square brackets [] (using the -f option with ps). kthreadd spawned processes will have a PPID of 2.

    -The kernel starts /sbin/init which is the parent/grandparent of all user processes.
#### Commands
ps -elf <-- static  option --forest
**htop** <-- dynamic  
sudo ls -l /proc/
ps -elf | grep sshd
ps --ppid 1 -lf | wc -l
sudo lsof -c sshd



### Ownership  
-The effective user ID (euid), defines the access rights for a process. A second user ID, the real user ID   (ruid), indicates who initiated a process  
  +A user is an entity that can run processes and own files  

### System Calls  
-fork() When a process calls fork(), the kernel creates a nearly identical copy of the process  
-exec() When a process calls exec(program), the kernel starts program, replacing the current process  
  
### Signals   
kill -9  
kill {numbers 1-30 in fg send diff signals}  
  
### Orphans/Zombies
-Orphan - Parent process exited, adopted by sbin/init with PPID of 1
-All daemons are orphans
  +disown -a && exit # Close a shell/terminal and force all children to be adopted
-Zombie (defunct) - completed process, but still has an entry in the process table, waiting on parent to acknowledge or terminate
**all daemons are orphans but not all orphans are daemons**
**red teamers will make their process a daemon so it has a ppid of 1**

### Jobs
-‘jobs’ - Display List of Jobs running in the background

-‘fg’ or ‘bg’ - Push / Pull jobs to / from the foreground / background

-‘kill %<job number>’ - Terminate the process by job number

-‘[ctrl -z]’ or ‘kill -19’ - stop / suspend the job

-‘kill -9 <PID>’ or ‘pkill -9 <process name>’

m h dom mon dow user command

    -cron daemon checks the directories /var/spool/cron, /etc/cron.d and the file /etc/crontab, once a minute and executes any commands specified that match the time.

    -Two types of cron jobs

        +System cron jobs

            +run as root and rigidly scheduled

            +perform system-wide maintenance tasks (Cleaning out /tmp or rotating logs)

            +controlled by /etc/crontab

        -User cron jobs

            
            +Use 'crontab’ command to create user cron jobs

            +stored in /var/spool/cron/crontabs/

Cron Syntax

* * * * * /directory/and/command
| | | | |
| | | | +---- Day of the Week   (range: 0-7, 1 = Monday)
| | | +------ Month of the Year (range: 1-12)
| | +-------- Day of the Month  (range: 1-31)
| +---------- Hour              (range: 0-23)
+------------ Minute            (range: 0-59)


### File descriptors

# - The number in front of flag(s) is the file descriptor number used by the process associated with the file
u - File open with Read and Write permission
r - File open with Read permission
w - File open with Write permission
W - File open with Write permission and with Write Lock on entire file
mem - Memory mapped file, usually for share library

cat /lib/systemd/system/

# Windows Logging
- Getting user SID  
get-wmiobject win32_useraccount | select name,sid (PowerShell)  
Get-LocalUser | select Name,SID (PowerShell)  
wmic useraccount get name,sid (CMD.EXE ONLY)  
 
- **UserAssist** ~ tracks the GUI-based programs that were ran by a particular user  
  + View executable Files run  
Get-ItemProperty 'REGISTRY::HKEY_USERS\*\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count'  

  + View Shortcut files executed
Get-ItemProperty 'REGISTRY::HKEY_USERS\*\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}\Count'  

- **Windows Background Activity Moderator (BAM)** ~ Controls activity of background applications
Get-Itemproperty 'HKLM:\SYSTEM\CurrentControlSet\Services\bam\UserSettings\*' (Windows 1709 & 1803)  
Get-Itemproperty 'HKLM:\SYSTEM\CurrentControlSet\Services\bam\state\UserSettings\*' (Windows 1809 and newer)  

- **Recycle Bin** ~ very important, can help you accomplish a forensic investigation  
gci 'C:\$RECYCLE.BIN' -Recurse -Verbose -Force | select *  
gci 'C:\$RECYCLE.BIN' -Recurse -Force   

$R - Content of deleted files
$I - original PATH and name


- **Prefetch** ~ Prefetch files are created by the windows operating system when an application is run from a specific location for the first time  
  + gci -Path 'C:\Windows\Prefetch' -ErrorAction Continue | select * | select -first 5  
.pf

- **Jump Lists** ~ 7-10 taskbar (Jump List) is engineered to allow users to “jump” or access items they have frequently or recently used quickly and easily
  + gci -Recurse C:\Users\*\AppData\Roaming\Microsoft\Windows\Recent -ErrorAction Continue | select FullName, LastAccessTime
-Location in explorer
C:\%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations

- **Recent Files** ~ Registry Key that will track the last files and folders opened and is used to populate data in “Recent” menus of the Start menu
  + gci 'REGISTRY::HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs'
-get the extensions
  + Get-Item 'Registry::\HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.*' 
    Hive: \HKEY_USERS\S-1-5-21-1584283910-3275287195-1754958050-1005\Software\Microsoft\Windows\CurrentVersion\Explorer\Recent
    
- Convert File Hex to Unicode
  + [System.Text.Encoding]::Unicode.GetString((gp "REGISTRY::HKEY_USERS\*\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.txt")."0")
  
- Convert all of a users values from HEX to Unicode
  + Get-Item "REGISTRY::HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.txt" | select -Expand property | ForEach-Object { [System.Text.Encoding]::Default.GetString((Get-ItemProperty -Path "REGISTRY::HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.txt" -Name $_).$_)}

- **Browser Artifacts** ~ Stores details for each user account
- History will record the access to the file on the website that was accessed via a link.
  + .\strings.exe 'C:\Users\<username>\AppData\Local\Google\Chrome\User Data\Default\History'
  
- Find FQDNs in Sqlite Text Files
  + $History = (Get-Content 'C:\users\<username>\AppData\Local\Google\Chrome\User Data\Default\History')           -replace   "[^a-zA-Z0-9\.\:\/]",""

  + $History| Select-String -Pattern "(https|http):\/\/[a-zA-Z_0-9]+\.\w+[\.]?\w+" -AllMatches|foreach             {$_.Matches.Groups[0].Value}| ft

- Location
  + C:\Users\andy.dwyer\AppData\Local\Google\Chrome\User Data\Default\

-DEMO
  + .\strings.exe 'C:\users\andy.dwyer\AppData\Local\Google\Chrome\User Data\Default\History' -accepteula
  + strings.exe 'C:\users\andy.dwyer\AppData\Local\Google\Chrome\User Data\Default\Top Sites'
  + strings.exe  'C:\users\andy.dwyer\AppData\Local\Google\Chrome\User Data\Default\Login Data'


-**Event Logs**
Get-EventLog -LogName System -Newest 10
Get-EventLog -LogName System -Newest 3 | Format-Table -Wrap
Get-Eventlog -LogName Security | ft -wrap
Get-Eventlog -LogName Security | ft -wrap | findstr /i StR1nG 
Get-Winevent -FilterHashtable @{logname='Security';id='4624'} | ft -Wrap
(Get-WinEvent -Listlog *).count
Get-WinEvent -Listlog *
wevtutil gli security



# Linux Auditing and Logging
Journald ~ Systemd   
syslog ~ most other systems  

- Authentication  

  - /var/log/auth.log - Authentication related events  
  - /var/run/utmp - Users currently logged in  
  - /var/log/wtmp - History file for utmp  
  - /var/log/btmp - Failed login attempts  
        
- Application  

  - apache - Web Server (dir)  
  - apt - Package Manager (dir)  
  - /var/log/mysql.log  

- System

  - /var/log/messages - Legacy Catch all  
  - /var/log/syslog - Ubuntu/Debian Catch all  
  - dmesg = Device Messenger (queires /proc/kmsg)  
      - Kernel Ring Buffer - Never fills  
      - First logs generated by the system  























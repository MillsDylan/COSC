xfreerdp /v:10.50.29.185 /u:student +clipboard +dynamic-resolution +glyph-cache
http://10.50.21.56:8000/  
STACK# ~3  10.50.26.137 
DYMI-23-003
DYMI-23-003

INFO 
------------------------------------------------------------------------------------------------------------
5 min per question


Methods are things you can do to an object  
Properties are information about an object  
  
------------------------------------------------------------------------------------------------------------
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





































Unzip 1000 times
-
$trial = (gci -recurse $PATH -filter *.zip | Sort-Object LastAccessTime -Descending | Select-Object -First 1)

do {$trial |
  ForEach-Object {
  Expand-Archive $_.FullName -Force
  $trial = (gci -recurse $PATH -filter *.zip | Sort-Object LastAccessTime -Descending | Select-Object -First 1)
 }
 }
 until($trial | Where-Object{$_.Name -eq 'Omega'})

gci .\Omega1\Omega1.txt | Get-Content










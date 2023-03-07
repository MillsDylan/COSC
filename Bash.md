# Bash  
https://cted.cybbh.io/tech-college/pns/public/pns/latest/guides/bash_sg.html#_cut
Ctrl + r = reverse-i-search
#comment out tags on commands to search via rev-i-search
 curl cht.sh/{command}   
explainshell.com ---- # explains the command and options     
   
## Order of Evaluation
1. Redirection  
2. Alias  
3. Parameter expansion, command sub, arithmetic expantion, and quote removal  
4. Shell Function  
5. Built-In commands  
6. Hash tables  
7. Path variable  
8. Error {eg command not found}  
    
## Find 
```
find -mtime 2  
```
```
find -mmin -60  
```
```
find $HOME/1123 -iname "*.txt" ! -iname "*~*  
```
```
find $HOME/1123 -iname "*.txt" | egrep -v "*~.txt"   
```
```
find $HOME/1123 -iname "*[^~].txt"  
```
```
find $HOME/1123 -iname "*[^~].txt" -exec cp {} $HOME/CUT \ ;   
```
  
## sort   
<|____sort and uniq go together  {sort before you use uniq}   
sort -k2n <--- delimiter for column {filename}

## uniq   
```
<| -c{count}   -u {display only unique lines} -d {display only duplicate lines}   
```
## awk   
```
awk [options] 'selection _criteria {action}' input-file > output-file   
```
```
awk -F: '{print $1}'   
```
   - displays 1st field delimited by a ":"  
 ```
awk '{print $2}'
```
   - displays 2nd field: delimited automatically by whitespace  
 ```
 awk -F: '($3 == 0) {print $1}' /etc/passwd  
 ```
 $NF -last field  
 awk -v {-v option is to import bash variables} 
 ```
 egrep "/bin/false|/bin/bash" /etc/passwd | awk -F: '{OFS=":"}($7="/bin/bash"){print $1}'  
 ```
 ```
 echo "python is better than Bash" | awk '{print $NF, $2,$2,$3,$4,$1}'
 echo "python is better than Bash" | awk '{$1="Marines";$2="are";$5="soldiers",print}'
 ```
 ```
 awk -F: '($3 !~ "[0-3]") && ($7 == "/bin/bash") {print $1}' $HOME/passwd > $HOME/SED/names.txt  
 ```
 ```
 find / -iname "*.bin" 2>/dev/null | tail | awk 'BEGIN{FS="/";OFS="/"}{NF=NF-1;print $0}'
 ```
 
## sed --stream editor   
```
sed 's/abc/123/'    
```
   - Replace first occurrence of abc in every line with 123  
```
sed 's/abc/123/g'  
```
   - replace every occurrenc of abc in every line with 123  
 ```
sed '/hello/d'  
```
   - delethe the 'hello' lines. output everything else  
```
sed -i '<expression>' file.txt   
```
   -the - "sed inplace" to make changes permanent in file.txt  **cannot be a command piped to sed
   
```   
sed 's/FINDPATTERN/REPLACEPATTERN/g' #g is global   
 ```  
   
### conditionals
```
   #!/bin/bash
   
   if [[ condition ]]; then
      commands
   elif [[ condition ]]; then
      commands
   else
      commands
   fi
```
   ip reg
   ([0-9]{3}\.){3}[0-9]+
   
   
   
   
   
   
   
## Functions
  ```
   #!/bin/bash

hello_world () {
   echo 'hello, world'
}

hello_world
   ```
   ```
   sub5() {
   name="John"
   echo ${name:0:2}
      }
   
   $sub5
student@lin-ops:~$ Jo
   ```
 ## command substitution  
var=$(command)
   
   **Rev** --> reverses the output
 ```
 #!/bin/bash
 
 File=$1
 User=$2
 id=$3
 DIR="/home/$User"
 SHELLY='/bin/bash'
 lastline-$(tail -n1 $File)
 
 echo "$lastline" | awk -F: -v uu=$User -v ii=$id -v dd=$DIR -v ss=$SHELLY {'OFS=":"}{$1=uu;$3=$4=ii;$6=dd;$NF=ss;print}' 
 ```
 
 
 
 
 
 
  ## LOOPS
 
 ```
 #!/bin/bash

for i in {1..5}; 
do
    echo $i
done
 ```
 ```
 if [[ <condition> ]];then
  break
 fi
 ```  
  ``` 
   #!/bin/bash
counter=1
 while [[ $counter -le 10 ]]
 do 
  echo $counter
    ((counter++))
 done
 echo "All done!"
   ```
 ``` 
   #!/bin/bash
counter=1
 until [[ $counter -gt 10 ]]
 do 
  echo $counter
    ((counter++))
 done
 echo "All done!"
   ```
 #### Range Loop
 ```
   range () {
     for value in {1..5}
  do  
      echo $value
 done
 echo "all done!"
 }
 range  
```  
### Range loop that counts down  
 ```  
 rocket () {  
 for value in {10..1}  
 do   
      echo $value  
      sleep 1  
 done   
 echo "team rocket blasts off again" 
 }
 ```  

                    

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
# CHALLENGES

Using Brace-Expansion, create the following directories within the $HOME directory:
1123,1134,1145,1156 
```
 mkdir $HOME/11{23,34,45,56}
 ```
 Use Brace-Expansion to create the following files within the $HOME/1123 directory. You may need to create the $HOME/1123 directory. Make the following files, but utilze Brace Expansion to make all nine files with one touch command.:
 1.txt,2.txt,3.txt,4.txt,5.txt,6~.txt,7~.txt,8~.txt,9~.txt
 ```
 touch $HOME/1123/{1,2,3,4,5,6~,7~,8~,9~}.txt
 ```
 Using the find command, list all files in $HOME/1123 that end in .txt.
 ```
 find $HOME/1123/*.txt
 ```
 List all files in $HOME/1123 that end in .txt. Omit the files containing a tilde (~) character.
 ```
 find $HOME/1123/*.txt | egrep -iv *~.txt*
 ```
 Copy all files in the $HOME/1123 directory, that end in ".txt", and omit files containing a tilde "~" character, to directory $HOME/CUT.
Use only the find and cp commands. You will need to utilize the -exec option on find to accomplish this activity.
 ```
 cp --copy-content $HOME/1123/{1..9}.txt $HOME/CUT/
 ```
 Using ONLY the find command, find all empty files/directories in directory /var and print out ONLY the filename (not absolute path), and the inode number, separated by newlines.
123 file1
456 file2
789 file3
 ```
 find /var -empty -printf '%i %f\n'
 ```
 Using ONLY the find command, find all files on the system with inode 4026532575 and print only the filename to the screen, not the absolute path to the file, separating each filename with a newline. Ensure unneeded output is not visible.
```
find -inum 4026532575 '%f/n'
``` 
 Using only the ls -l and cut Commands, write a BASH script that shows all filenames with extensions ie: 1.txt, etc., but no directories, in $HOME/CUT.
Write those to a text file called names in $HOME/CUT directory.
Omit the names filename from your output.
 ```
 ls -l $HOME/CUT | cut -d. -f1- -s | cut -d: -f2 | cut -d' ' -f2 > $HOME/CUT/names
 ```
 Write a basic bash script that greps ONLY the IP addresses in the text file provided (named StoryHiddenIPs in the current directory); sort them uniquely by number of times they appear.
 ```
 egrep -o '([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})\.([0-9]{1,3})' StoryHiddenIPs | sort | uniq -c | sort -rn
``` 
 Using ONLY the awk command, write a BASH one-liner script that extracts ONLY the names of all the system and user accounts that are not UIDs 0-3.
Only display those that use /bin/bash as their default shell.
The input file is named $HOME/passwd and is located in the current directory.
Output the results to a file called $HOME/SED/names.txt
``` 
 awk -F: '($3 > 3) && ($7 == "/bin/bash") {print $1}' $HOME/passwd > $HOME/SED/names.txt
 ```
 Find all dmesg kernel messages that contain CPU or BIOS (uppercase) in the string, but not usable or reserved (case-insensitive)
Print only the msg itself, omitting the bracketed numerical expressions ie: [1.132775]
``` 
dmesg | egrep 'CPU|BIOS' | egrep -iv 'usable|reserved' | cut -d"]" -f2- 
``` 
Write a Bash script using "Command Substitution" to replace all passwords, using openssl, from the file $HOME/PASS/shadow.txt with the MD5 encrypted password: Password1234, with salt: bad4u
Output of this command should go to the screen/standard output.
You are not limited to a particular command, however you must use openssl. Type man openssl passwd for more information. 
``` 
 HASHED=$(openssl passwd -1 -salt bad4u Password1234) 
awk -F: -v hh=$HASHED {'OFS=":"}{$2=hh;print}' $HOME/PASS/shadow.txt
``` 
 Using ONLY sed, write all lines from $HOME/passwd into $HOME/PASS/passwd.txt that do not end with either /bin/sh or /bin/false.
 ```
 sed '/\/bin\/sh/d;/\/bin\/false/d' $HOME/passwd > $HOME/PASS/passwd.txt
``` 
 Using find, find all files under the $HOME directory with a .bin extension ONLY.
Once the file(s) and their path(s) have been found, remove the file name from the absolute path output.
Ensure there is no trailing / at the end of the directory path when outputting to standard output.
You may need to sort the output depending on the command(s) you use. Have each path displayed only once.
 ```
 find $HOME -name '*.bin' -printf "%h\n" | sort | uniq
 ```
 Write a script which will copy the last entry/line in the passwd-like file specified by the $1 positional parameter
Modify the copied line to change:
User name to the value specified by $2 positional parameter
Used id and group id to the value specified by $3 positional parameter
Home directory to a directory matching the user name specified by $2 positional parameter under the /home directory (i.e. if the $2 was 'Chris', the new line would have /home/Chris as its home directory)
The default shell to `/bin/bash'
Append the modified line to the end of the file
``` 
 tail -1 $1 | awk -F: -v tt=$2 -v hh=$3 '{OFS=":"}{$1=tt;$3=$4=hh;$6="/home/"tt;$NF="/bin/bash";print$0}' >> $1
``` 
 Find all executable files under the following four directories:
/bin
/sbin
/usr/bin
/usr/sbin
Sort the filenames with absolute path, and get the md5sum of the 10th file from the top of the list.
 ```
 heat=$(find /bin /sbin /usr/bin /usr/sbin -type f -executable | sort | awk 'NR==10 {print $0}')
md5sum $heat | cut -d" " -f1
 ```
 Sort the /etc/passwd file numerically by the GID field.
For the 10th entry in the sorted passwd file, get an md5 hash of that entry’s home directory.
Output ONLY the MD5 hash of the directory's name to standard output.
``` 
sort -t":" -k4n /etc/passwd | awk 'NR==10 {print$0}' | cut -d":" -f6 | md5sum | cut -d" " -f1 
``` 
 Write a script which will find and hash the contents 3 levels deep from each of these directories: /bin /etc /var
Your script should:
Exclude file type named pipes. These can break your script.
Redirect STDOUT and STDERR to separate files.
Determine the count of files hashed in the file with hashes.
Determine the count of unsuccessfully hashed directories.
Have both counts output to the screen with an appropriate title for each count.
```
 find /bin /etc /var -maxdepth 3 ! -type p -exec md5sum {} \; 1> good.txt 2> bad.txt
no=$(egrep "directory" bad.txt | wc -l | cut -d" " -f1)
yes=$(wc -l good.txt | cut -d" " -f1)
echo "Successfully Hashed Files: $yes"
echo "Unsuccessfully Hashed Directories: $no"
```
Design a script that detects the existence of directory: $HOME/.ssh
Upon successful detection, copies any and all files from within the directory $HOME/.ssh to directory $HOME/SSH and produce no output. You will need to create $HOME/SSH.
Upon un-successful detection, displays the error message "Run ssh-keygen" to the user.
``` 
if [ -d "$HOME/.ssh" ] ;
	then cp -a $HOME/.ssh/. /$HOME/SSH/ ;
else echo "Run ssh-keygen"
fi 
``` 
Write a script that determines your default gateway ip address. Assign that address to a variable using command substitution.
NOTE: Networking on the CTFd is limited for security reasons. ip route and route are emulated. Use either of those with no arguments/switches.
Have your script determine the absolute path of the ping application. Assign the absolute path to a variable using command substitution. HINT: man which
Have your script send six ping packets to your default gateway, utilizing the path discovered in the previous step, and assign the response to a variable using command substitution.
Evaluate the response as being either successful or failure, and print an appropriate message to the screen. 
 ```
 A=$(ip route | awk '{print $3}' | head -1)
B=$(which ping)
C="$B -c 6 $A"
D=$($C | grep '0%' | awk '{print $4}' )
if [[ "$D" == '6' ]]; then
   echo "successful";
else
   echo "failure";
fi
 ```
Create the following files in a new directory you create $HOME/ZIP:
file1 will contain the md5sum of the text 12345
file2 will contain the md5sum of the text 6789
file3 will contain the md5sum of the text abcdef
Create a zip file containing the three files above, without being stored inside a directory in the zip file. Name the zip file $HOME/ZIP/file.zip
HINT: "Junk" the paths
Utilize tar on $HOME/ZIP/file.zip to archive it into a file called $HOME/ZIP/file.tar.gz which should not include directories. Use the gzip option in tar, DO NOT use a seperate gzip binary. 
``` 
 mkdir $HOME/ZIP
cd $HOME/ZIP
echo '12345' | md5sum | awk '{print $1}' > file1
echo '6789' | md5sum | awk '{print $1}' > file2
echo 'abcdef' | md5sum | awk '{print $1}' > file3
zip -r file.zip file1 file2 file3
tar -cvzf file.tar.gz file.zip
``` 
Utilize Bash looping to iteratively create each user account and their directories.
Design a basic FOR Loop that iteratively alters the file system and user entries in a passwd-like file for new users: LARRY, CURLY, and MOE.
Each user should have an entry in the $HOME/passwd file
The userid and groupid will be the same and can be found as the sole contents of a file with the user's name in the $HOME directory (i.e. $HOME/LARRY.txt might contain 123)
The home directory will be a directory with the user's name located under the $HOME directory (i.e. $HOME/LARRY)
NOTE: Do NOT use shell expansion when specifying this in the $HOME/passwd file.
The default shell will be /bin/bash
The other fields in the new entries should match root's entry
Users should be created in the order specified
 ```
 #!/bin/bash
rootline=$(egrep "root" $HOME/passwd)
for x in LARRY CURLY MOE; do
  ID=$(cat $HOME/$x.txt)
  mkdir $HOME/$x
  echo "$rootline" | awk -F: -v uu=$x -v ii=$ID '{OFS=":"}{$1=uu;$3=$4=ii;$6="$HOME/"uu;$NF="/bin/bash";print}' >> $HOME/passwd
done
``` 
Write a bash script that will find all the files in the /etc directory, and obtains the octal permission of those files. The stat command will be useful for this task.
Depending how you go about your script, you may find writing to the local directory useful. What you must seperate out from the initial results are the octal permissions of 0-640 and those equal to and greater than 642, ie 0-640 goes to low.txt, while 642 is sent to high.txt.
Have your script uniquely sort the contents of the two files by count, numerically-reversed, and output the results, with applicable titles, to the screen. An example of the desired output is shown below.
NOTE: There is a blank line being printed between the two sections of the output below.
``` 
 find /etc -type f -printf '%m\n' 2>/dev/null > initial.del
for OCTAL in $(cat initial.del)
do
    if [[ $OCTAL -ge 642 ]];then
        echo "$OCTAL" >> high.del
    elif [[ $OCTAL -le 640 ]];then
        echo "$OCTAL" >> low.del
    fi
done
echo "Files w/ OCTAL Perm Values 642+:"
cat high.del | sort| uniq -c | sort -nr
echo ""
echo "Files w/ OCTAL Perm Values 0-640:"
cat low.del | sort |uniq -c | sort -nr
rm *.del
 ```
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

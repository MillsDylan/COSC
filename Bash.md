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
find -mtime 2  
find -mmin -60  
find $HOME/1123 -iname "*.txt" ! -iname "*~*  
find $HOME/1123 -iname "*.txt" | egrep -v "*~.txt"   
find $HOME/1123 -iname "*[^~].txt"    
find $HOME/1123 -iname "*[^~].txt" -exec cp {} $HOME/CUT \ ;   
  
## sort   
<|____sort and uniq go together  {sort before you use uniq}   
sort -k2n <--- delimiter for column {filename}

## uniq   
<| -c{count}   -u {display only unique lines} -d {display only duplicate lines}   
## awk   
awk [options] 'selection _criteria {action}' input-file > output-file   
awk -F: '{print $1}'   
   - displays 1st field delimited by a ":"  
awk '{print $2}'  
   - displays 2nd field: delimited automatically by whitespace  
 awk -F: '($3 == 0) {print $1}' /etc/passwd  
 $NF -last field  
 awk -v {-v option is to import bash variables}   
 egrep "/bin/false|/bin/bash" /etc/passwd | awk -F: '{OFS=":"}($7="/bin/bash"){print $1}'        
 echo "python is better than Bash" | awk '{print $NF, $2,$2,$3,$4,$1}'
 echo "python is better than Bash" | awk '{$1="Marines";$2="are";$5="soldiers",print}'
 awk -F: '($3 !~ "[0-3]") && ($7 == "/bin/bash") {print $1}' $HOME/passwd > $HOME/SED/names.txt  
 find / -iname "*.bin" 2>/dev/null | tail | awk 'BEGIN{FS="/";OFS="/"}{NF=NF-1;print $0}'
 
 
## sed --stream editor   

sed 's/abc/123/'    
   - Replace first occurrence of abc in every line with 123  
sed 's/abc/123/g'  
   - replace every occurrenc of abc in every line with 123  
sed '/hello/d'  
   - delethe the 'hello' lines. output everything else  
sed -i '<expression>' file.txt   
   -the - "sed inplace" to make changes permanent in file.txt  **cannot be a command piped to sed
sed 's/FINDPATTERN/REPLACEPATTERN/g' #g is global   
   
   
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
 
 ```
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
 
   
   

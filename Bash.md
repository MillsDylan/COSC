# Bash  
https://cted.cybbh.io/tech-college/pns/public/pns/latest/guides/bash_sg.html#_cut
Ctrl + r = reverse-i-search
#comment out tags on commands to search via rev-i-search
   
## Order of Evaluation
1. Redirection  
2. Alias  
3. Parameter expansion, command sub, arithmetic expantion, and quote removal  
4. Shell Function  
5. Built-In commands  
6. Hash tables  
7. Path variable  
8. Error {eg command not found}  
  
curl cht.sh/{command}   
explainshell.com ---- # explains the command and options   
  
  **Find Find Find Find Find Find Find Find Find Find Find Find**   
find -mtime 2  
find -mmin -60  
find $HOME/1123 -iname "*.txt" ! -iname "*~*  
find $HOME/1123 -iname "*.txt" | egrep -v "*~.txt"   
find $HOME/1123 -iname "*[^~].txt"    
find $HOME/1123 -iname "*[^~].txt" -exec cp {} $HOME/CUT \ ;   
  
  
- man pages
- absolute/relative paths
- mkdir,touch,ls,rm,rmdir,cp,mv
- cat,more,less
- find,grep,brace expansion
- top,ps,free,kill,killall
- cut
- commmand chaining operators
- output redirection  
  
**conditionals  **
**command substitution**  
**commands  **
**alais  **
**sort**  <|____sort and uniq go together  {sort before you use uniq}   
**uniq**  <| -c{count}   -u {display only unique lines} -d {display only duplicate lines}   
**awk**  awk [options] 'selection _criteria {action}' input-file > output-file   
awk -F: '{print $1}'   
   - displays 1st field delimited by a ":"  
awk '{print $2}'  
   - displays 2nd field: delimited automatically by whitespace  
 awk -F: '($3 == 0) {print $1}' /etc/passwd  
   
**sed --stream editor**   
sed 's/abc/123/'    
   - Replace first occurrence of abc in every line with 123  
sed 's/abc/123/g'  
   - replace every occurrenc of abc in every line with 123  
sed '/hello/d'  
   - delethe the 'hello' lines. output everything else  
sed -i '<expression>' file.txt  
   -"sed inplace" to make changes permanent in file.txt  
   
   
   
   
   

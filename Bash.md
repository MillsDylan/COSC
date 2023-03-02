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
find $HOME/1123 -iname "*[^~].txt" -exec cp {} $HOME/CUT \;

  
- man pages
- absolute/relative paths
- mkdir,touch,ls,rm,rmdir,cp,mv
- cat,more,less
- find,grep,brace expansion
- top,ps,free,kill,killall
- cut
- commmand chaining operators
- output redirection  
  
conditionals  
command substitution  
commands  
alais  
sort  <|____sort and uniq go together  
uniq  <|  
awk  
sed  



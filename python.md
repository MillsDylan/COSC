
##https://cctc.cybbh.io/students/students/latest/Day_0_Setup.html


#REPL 
student@xxx ~ python3
or 
specify infront of command 
or 
in VIM #!/usr/bin/python3

#lists, tuples

#index starts furhtest to the left at 0

##Converting
int(myvar)
float(myvar)
str(myvar)
newvar = int(myvar)

#30 / 3 = 10.0 ~ float --- division
30 // = 10 ~ int --- integer division
2 ** 2 --- exponents


#.format(,)
name = dylan 

list(name)
d,y,l,a,n

#"".join(name)

#.split()

"192.100.10.90".split(".")[2]
	10

#"dylan.mills@usmc.mil".replace("@",".")
'dylan.mills.usmc.mil

#"       hello      ".strip()
'hello'

#"dylan".strip(d)
'ylan'


#len("aslkdf;lasjf;lasd")
 25
 
 
 script
 name = input("please enter your name: ")
   age =input("please enter age:")
   
   




exercise 04 

name = "Jerry"
greeting = "Sir"
time = "noon"
print(Hello {}! {}, will you be arriving by {}? .format(name,greeting,time))





DAY 2

if age >= 21:
	print('you can do it')
elif age < 21:
	print('sorry you cant')


	
import datetime

x = datetime.datetime.now()

print(x.strftime("%c"))
	
	
  FOR LOOPS
  
  for item in iterable:
  	do stuff
	

list(range(0,10,2))
	range(start,stop,step)
	
List slicing
sequence[start:stop:step


List Comprehension
my_list = [ valueExpression for controlVariable in iterator whereClause ]

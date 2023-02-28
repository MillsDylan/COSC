# PYTHON
https://cctc.cybbh.io/students/students/latest/Day_0_Setup.html


##### REPL-Read Evaluate Print Loop
-Enter command "python3" or In VIM #!/usr/bin/python3

+ bool - boolean (True or False)  
+ int - integers  
+ float - floating point decimal  
+ str - string  
+ tuple - immutable sequence of items (not necessarily of the same type)  
+ list - mutable sequence of items (not necessarily of the same type)  

	##### *The index value starts furthest to the left at 0*

## Type Conversion
- int(myvar)  
- float(myvar)  
- str(myvar)  
- newvar = int(myvar)  

## Math  
30 / 3 = 10.0 ~ float --- division  
30 // = 10 ~ int --- integer division  
2 ** 2 --- exponents  

## String Manipulation
**.format()**  
```
name = "Jerry"  
greeting = "Sir"  
time = "noon"  
print(Hello {}! {}, will you be arriving by {}? .format(name,greeting,time))  
```

**.split()**
```
>>> 'user:passwd'.split(':')  
['user', 'passwd']
```  
```  
>>>'dylan.mills@usmc.mil'.replace('@','.')
>>>'dylan.mills.usmc.mil'.split('.')
'dylan, mills, usmc, mil
```  
## List Modiication
*Creating varable containing contents of a list  (after being split)*
```
>>> l = list(s)  
>>> l  
['h', 'e', 'l', 'l', 'o']  
```
hello >> jello   --- *this can occur because lists are mutable(strings are not mutable)*
```
>>> l[0] = 'j'
>>> l
['j', 'e', 'l', 'l', 'o']
```
Create list
```
>>> a = [1,2,3,4,5]
```
Swap first and last element
```
>>> a[0], a[-1] = a[-1], a[0]
>>> a
[1, 2, 3, 4, 5]
```



**.join()**
```
>>> ''.join(l)
'jello'
>>> '.'.join(l)   # Var used is from Ex. above
'j.e.l.l.o'
>>> '::'.join(l)  # The separator can be multiple characters
'j::e::l::l::o'
```
**.strip**
```
>>>"       hello      ".strip()
'hello'
```
```
"dylan".strip(d)
'ylan'
```  
**Print() multiple objects**
```
>>> a = 'hello'
>>> b = 'world'
>>> print(a, b)
hello world
>>> print(a, b, sep=':')
hello:world
```
**Print() without a newline**
```
>>> print('hello'); print('world')
hello
world
>>> print('hello',end=''); print('world')
helloworld
>>> print('hello',end=' '); print('world')
hello world
```
**input()**
```
>>> name = input('What is your name? ') # blocks for keyboard input
>>> name
'whatever the user typed'
```
## Branching  
```  
if <condition>:
    <indented code block>
elif <condition>:
    <indented code block>
elif <condition>:
    <indented code block>
else:
    <indented code block>  
 ```  
 
| **Comparison Operators**  |        |
| ------------- | ------------- |
| equal to  | ==  |
| not equal to  | !=  |
| less than | > |
| Less than or equal | <= |
| greater that | > |
| greater than or equal | >= |
| value in collection | in |
| object id match | is |

| **Logical Operators** |    |
| ----------------- | -- |
| logical AND | and |
| logical OR | or |
| logical NOT | not |  

**Defining a Function**
```
def multiply(a, b):
  return a * b
```
## Loops
**While**
```
a = 0
while a < 10:
  print(a)
  a += 1
```  
**For**
```
for item in <iterable>:
  <code block>
```
**Range**
```
for i in range(0, 11, 2):
  print(i)
```
**Slicing**
-Slices are similar to using an index on a sequence, except that it can encompass more than one item.  
```
sequence[start:stop:step]
```
Reverse sequence
```
primes[::-1]
```
## List Comprehension
*Come back to this topic in free time*

## Bit manipulation
**Bin() Format()**
```
>>> a = 42
----------------
>>> bin(a)
'0b101010'
----------------
>>> format(a,'#b') ---> *'#'Gives a prefix*
'0b101010'
----------------
>>> format(a,'b') ---> *no prefix*
'101010'
----------------
>>> format(a,'0>8b') ---> *converted to a binary string, zero-filled, right aligned, width of 8*
'00101010'
```
Add 1 to the integer 'a'
```
>>> l = list(bin(a))
>>> l[-1] = '1'
>>> a = int(''.join(l),base=2) ---> binary string literal, since its a string, base must be specified
>>> a
43
```
**ord()** -- turn char into decimal value
```
>>>ord(a)
97
```
## Dictionary
```
myDict = {}
for i in myStr:
	if i in myDict:
		myDict[i] += 1 
	else: 
		myDict[i] = 1
```

**variable length argument**
	def calc_you_later(*args):
		return sum(args)

###  File IO
```
with open('test.txt','w) as fp:
```
```
lines='hello\n','name\n','is\n'
fp.writelines(lines)
hello
name
is
```
**adding a newline terminator**
```
for item in myVar:
	fp.write(str(item)+'\n')
```
copying to a file
```
with open('test.txt') as source, open ('copy.txt', 'w') as dest:
	dest.write(source.read())       ----> reading source file and writing to source file
```

## counting duplicates
	
```	
	string = 'klasjdf;lasj;flsjda'
	newlist = list(string)
	mydict = {}
	
	for x in newlist:
		if x not in mydict:
			mydict[x] = 1
		else:
			mydict[x] =+ 1
	print(mydict)
```


.isdigit()










# 10.15. Chapter Assessment

**The textfile, travel_plans.txt, contains the summer travel plans for someone with some commentary.   
Find the total number of characters in the file and save to the variable num.**
```
with open ('travel_plans.txt', 'r') as fp:
    num = len(fp.read())
 ``` 
 **We have provided a file called emotion_words.txt that contains lines of words that describe emotions. Find the total number of words in the file and assign this value to the variable num_words.**
 ```
 with open ('emotion_words.txt', 'r') as fp:
    w = fp.read()
    r = w.split()
    num_words = len(r)
 ```
 **Assign to the variable num_lines the number of lines in the file school_prompt.txt.**
 ```
 with open ('school_prompt.txt', 'r') as fp:
    lines = fp.readlines()
    num_lines = len(lines)
 ```
 **Assign the first 30 characters of school_prompt.txt as a string to the variable beginning_chars.**
 ```
 with open ('school_prompt.txt', 'r') as fp:
    beginning_chars = fp.read(30)
 ```
 **Challenge: Using the file school_prompt.txt, assign the third word of every line to a list called three.**
 ```
 three = []
with open ('school_prompt.txt', 'r') as fp:
    for i in fp.readlines():
            three.append(i.split()[2])
```
**Challenge: Create a list called emotions that contains the first word of every line in emotion_words.txt.**
```
emotions = []
with open ('emotion_words.txt', 'r') as fp:
    for i in fp.readlines():
            emotions.append(i.split()[0])
```
**Assign the first 33 characters from the textfile, travel_plans.txt to the variable first_chars.**
```
with open ('travel_plans.txt', 'r') as fp:
        first_chars = fp.read()[:33]
```
**Challenge: Using the file school_prompt.txt, if the character ‘p’ is in a word, then add the word to a list called p_words.**
```
p_words = []
with open ('school_prompt.txt', 'r') as fp:
    lines = fp.read()
    for i in lines.split():
            if 'p' in i:
                    p_words.append(i)
                    
```
Write a function that takes in a string of one or more words, and returns the same string, but with all five or more letter words reversed (Just like the name of this Kata). Strings passed in will consist of only letters and spaces. Spaces will be included only when more than one word is present.

Examples:
```
spinWords( "Hey fellow warriors" ) => returns "Hey wollef sroirraw" 
spinWords( "This is a test") => returns "This is a test" 
spinWords( "This is another test" )=> returns "This is rehtona test"
```
Finished work
```
def spin_words(sen):
    return(str(' '.join([i[::-1]  if (len(i) >= 5) else i for i in sen.split(' ')])))
```      

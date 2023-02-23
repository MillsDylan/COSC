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

### Math  
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

**Creating varable containing contents of a list  (after being split)**
```
>>> l = list(s)  
>>> l  
['h', 'e', 'l', 'l', 'o']  
```
**Modifying the list**
```
>>> l[0] = 'j'
>>> l
['j', 'e', 'l', 'l', 'o']
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

**Defining Function** 
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
**List Comprehension**
*Come back to this topic in free time*

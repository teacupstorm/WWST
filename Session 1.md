## Agenda

- [ ] Introduction.
- [ ] decompilation of souce code.
- [ ] logging (debugging) mysql quieries.
- [ ] understanding vulnerability discovery.
- [ ] tools ==> DnSpy & JD-Gui


## Introduction

Robensive **Whitebox Web-application Security Testing** (WWST) is a  course which is designed to find vulnerabilities in web application through white or grey box testing methodology. This course will help you to get more confidence over web vulnerability discovery and exploitation. 

## decompilation of souce code.

Decompilation is process to get source code from the executable or binary to do a source code vulnerability discovery. In this process we will be using some tools to get source code from the .net and java executable.

## logging (debugging) mysql quieries.

To find vulnerability related to mysql like SQL-Injection logging quiries are important to do a greybox testing to the functionality. by just logging and reading logs for mysql critical vulnerability such as sql injeciton can be discoverd.

Configuration of mysql config file is important so that we will able to see and debug quiries realtime.
	
```batch
#windows (XAMPP)
C:\xampp\mysql\bin\my.ini
==> add below lines
	general_log = 1
	general_log_file = "mysql_query.log"

```

```batch
#linux
/etc/mysql/my.cnf
==> add below lines
	general_log = 1
	general_log_file = "/var/log/mysql/mysql.log"
```

to view (read mysql log)
```batch
#windows
install git bash 
open gitbash in mysql log file location 
tail mysql_query.log
#for watch use below script 
save it to /bin/watch ; add execution privilege

```
```bash
#!/bin/bash
ARGS="${@}"
clear; 
while(true); do 
  OUTPUT=`$ARGS`
  clear 
  echo -e "${OUTPUT[@]}"
done
```

```bash
#linux
watch tail /var/log/mysql/mysql.log
```


## understanding vulnerability discovery.

To discover vulnerability from souce code is not easy task as souce code can contains thousands of lines of code and many functions. its not easy to start discovering vulnerability by reading source code as you will super confused with a questing ...where to start....!!? 

To start off vulnerability discovery its better to go for greybox approch so we can target functions according to web functionality.

the best approch to discover vulnerability will be

                    
```flow
st=>start: Login
op=>operation: Login operation
cond=>condition: Successful Yes or No?
e=>end: To admin

st->op->cond
cond(yes)->e
cond(no)->op
```
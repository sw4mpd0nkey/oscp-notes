## Basic SQL Reminders

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
## Basic SQL Reminders

Using the mysql command, we'll connect to the remote SQL instance by specifying root as username and password, along with the default MySQL server port 3306.
> mysql -u root -p'root' -h 192.168.50.16 -P 3306

We can run impacket-mssqlclient to connect to the remote Windows machine running MSSQL by providing a username, a password, and the remote IP, together with the -windows-auth keyword. 
> impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
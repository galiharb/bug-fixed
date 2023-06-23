# [Fixed] Bad Gateway Nginx Proxy Manager
## Permission Denied create table `migrations`

### Failed Login because Bad Gateway [Temporary Fixed]
The Problem : <br>
<img src="https://i.ibb.co/mvG8PJr/image.png" alt="image" border="0">

The log : <br>
<img src="https://i.ibb.co/TLv9Trz/error.png" alt="error" border="0">

See what a user use mysql : <br>
<img src="https://i.ibb.co/zRzMV7Q/1.png" alt="1" border="0">
<br>
Fail to use database because root is owner of mysql, so you must change it

I am use docker, so u can follow this command <br>
<img src="https://i.ibb.co/PMMfRHx/2.png" alt="2" border="0">
```
docker exec -it <id container> chown -R mysql:mysql /var/lib/mysql
```

After that, you can check the log again, and the problem has gone : <br>
<img src="https://i.ibb.co/TMwzd19/hasil.png" alt="hasil" border="0">


### Permanently Solved the bug [Alternative]


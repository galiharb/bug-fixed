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
restart your nginx proxy and nginx db, if you use docker-compose you can use
```
docker-compose restart
```

After that, you can check the log again, and the problem has gone : <br>
<img src="https://i.ibb.co/TMwzd19/hasil.png" alt="hasil" border="0">


## Permanently Solved the bug [Alternative]

Not use mysql, convert that to sqllite

1. Export the database
```
docker exec -it <container id db nginxproxy> mysqldump --user=npm --password=<your_password> npm -h 127.0.0.1 > npm-export.sql
```

2. clone mysql to sqllite tools
```
git clone https://github.com/dumblob/mysql2sqlite.git .
```

3. convert
```
./mysql2sqlite npm-export.sql | sqlite3 database.sqlite
```

4. change your docker-compose, you can use my config :
```
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_SQLITE_FILE: /data/database.sqlite
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
      - ./data/database.sqlite:/data/database.sqlite
```

Congrats !!! The bug will not show again


reference :
https://github.com/NginxProxyManager/nginx-proxy-manager/issues/310

##  frps docker
frps.ini place in ./conf/frps.ini

###  run
```
docker run -d --name frps --network host    blueblue/frps
```

```
or run with coustom configuration:
docker run -d --name frps --network host -v  your_frps_ini_path:/conf/frps.ini   blueblue/frps
```

### defualt config:
```
bind_port = 7000

dashboard_port = 7500

dashboard_user = admin

dashboard_pwd = admin

privilege_token = admin10086
```

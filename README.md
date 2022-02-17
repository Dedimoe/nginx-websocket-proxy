#nginx-websocket-proxy
Nginx as websocket proxy

Let assume, you have nodejs websocket server run on 3001 port,
 
```
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }
 
    upstream websocket {
        server 127.0.0.1:3001;
    }
 
    server {
        listen 8180;
        location / {
            proxy_pass http://websocket;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
            proxy_set_header Host $host;
        }
    }
}
```

Test using wscat :
```
wscat --connect ws://localhost:8180
output: Connected (press CTRL+C to quit)
>
```

Just it.


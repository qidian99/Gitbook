# 个人网站

神Nginx配置奇的

```text
 24           location / { 
 25               # index  index.html index.htm; 
 26               # proxy_pass http://localhost:5555; 
 27               try_files $uri /index.html =404; 
 28           }
```


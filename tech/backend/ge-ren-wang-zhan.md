# 个人网站

神奇的Nginx配置

```text
 24           location / { 
 25               # index  index.html index.htm; 
 26               # proxy_pass http://localhost:5555; 
 27               try_files $uri /index.html =404; 
 28           }
```

## try\_files

{% embed url="https://www.nginx.com/resources/wiki/start/topics/tutorials/config\_pitfalls/\#:~:text=If%20you%20have%20any%20recent,just%20made%20life%20much%20easier.&text=What%20we%20changed%20is%20that,exist%20try%20a%20fallback%20location" %}

BAD:

```text
server {
    root /var/www/example.com;
    location / {
        if (!-f $request_filename) {
            break;
        }
    }
}
```

GOOD:

```text
server {
    root /var/www/example.com;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

What we changed is that we try to see if `$uri` exists without requiring `if`. Using `try_files` means that you can test a sequence. If `$uri` doesn’t exist, try `$uri/`, if that doesn’t exist try a fallback location.

In this case, if the `$uri` file exists, serve it. If not, check if that directory exists. If not, then proceed to serve `index.html` which you make sure exists. It’s loaded – but oh-so-simple! This is another instance where you can completely eliminate If.  





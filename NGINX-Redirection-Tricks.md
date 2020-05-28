
### 1# Redirect to another domain & Permenant Redirect to new URL
```
server {
  server_name .mydomain.com;
 return 301 http://example.com$request_uri;
}
```
Or
```
server {
  server_name .mydomain.com;
  return 301 http://www.adifferentdomain.com$request_uri;
}
```
Or
```
server {
# Permanent redirect to new URL
server_name olddomain.com;
rewrite ^/(.*)$ http://newdomain.com/$1 permanent;
}
```

### 2# Redirect HTTP to HTTPS
```
server {
# Redirect to HTTPS
listen      80;
server_name domain.com www.domain.com;
return      301 https://example.com$request_uri;
}
```


### 3# Rewrite missing http://

```
server {
  server_name .mydomain.com;
rewrite ^ http://example.com permanent;
}
```

### 4# Return statement position is at topmost context e.g. below

```
server {
    location /a/ {
        try_files test.html =404;
    }

    location / {
        return 301 http://example.org;
    }
}
```


### 5# Permanent www to non-www Redirect
```
server {
server_name www.domain.com;
rewrite ^/(.*)$ http://domain.com/$1 permanent;
}
```

### 6# Permanent Redirect to www
```
server {
server_name domain.com;
rewrite ^/(.*)$ http://www.newdomain.com/$1 permanent;
}
```
### 7# Proxy Everything

```
    server_name _;
    root /var/www/site;
    location / {
        try_files $uri $uri/ /index.php;
    }
    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_pass unix:/tmp/phpcgi.socket;
    }
}
```

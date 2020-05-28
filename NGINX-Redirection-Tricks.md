
### 1# Redirect to another domain
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

### 2# Rewrite missing http://

```
server {
  server_name .mydomain.com;
rewrite ^ http://example.com permanent;
}
```

### 3# Return statement position is at topmost context e.g. below

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
### 4# Proxy Everything

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

### Let us first defind the redirection types 
#### Temporary redirects
######  (response status code 302 Found) are useful if a URL temporarily needs to be served from a different location. For example, if you are performing site maintenance, you may wish to use a temporary redirect of from your domain to an explanation page to inform your visitors that you will be back shortly.
#### Permanent redirects
######  (response status code 301 Moved Permanently), on the other hand, inform the browser that it should forget the old address completely and not attempt to access it anymore. These are useful when your content has been permanently moved to a new location, like when you change domain names.

##### You can create a temporary redirect in Nginx by adding a line like this to the server block entry in the server configuration file:
```
rewrite ^/oldlocation$ http://www.newdomain.com/newlocation redirect;
```
Similarly, use a line like this for a permanent redirect:
```
rewrite ^/oldlocation$ http://www.newdomain.com/newlocation permanent;
```

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
### 7# You can also combine multiple domain names in the server parameter
```
server {
  server_name example.com www.example.com;
    rewrite ^/products.html$ /offer.html permanent;
    rewrite ^/services.html$ /offer.html permanent;
   }
```

### 8# Redirect to different Port
```
  server {
    listen 8080;

    location / {
      proxy_pass http://example.com:8787;
      proxy_redirect http://example.com:8787/ $scheme://$host:8080/;
    }
```
##### Note: $scheme is request scheme, “http” or “https”

### 9# Proxy Everything
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

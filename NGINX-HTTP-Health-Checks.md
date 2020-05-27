 ### Below are health checks that can be used in side the .http files in /etc/nginx/conf.d/*.http
 

 ### 1# Health Check Intervals & Attempts settings
 ```interval=10 will check every 10 seconds```
 ##### Explaination
 health_check uri=/index.html : This will check if index.html page is returing Status Code 200 Ok <br /><br />
 passes=2 backend servers must pass 2 checks.<br /><br />
 fails=3 if backend server failed 3 times it will be excluded from the pool
 ##### Usage
```
http {  
    #...  
    server {  
            listen  12345;  
            location / {
               proxy_pass http://backend;
               health_check uri=/index.html;
               health_check interval=10 fails=3 passes=2;
               
            }
    }  
    #... 
}  
```
### 2# Customize HTTP Status Code Health check with Range of Codes 
 ```   
  health_check match=server_ok; 
 ```
##### Explaination
health_check match=server_ok : You can set custom conditions that the response must satisfy for the server to pass the health check. The conditions are defined in a match block, which is referenced in the match parameter of the health_check directive.
##### Usage
###### In the HTTP Block insert below lines
```
http {
    #...
    match server_ok {
        status 200-399;
        body !~ "maintenance mode";
    }
  }
 ```
###### In /conf.d/*.http file mention the 
  health_check match=server_ok; 

```    
   server {
        #...
        location / {
            proxy_pass http://backend;
            health_check match=server_ok;
        }
    }
```

### 3# Customize HTTP Status Code Health check & Other headers
 ```   
  health_check match=welcome; 
 ```
##### Explaination
health_check match=welcome : You can set custom conditions that the response must satisfy for the server to pass the health check. The conditions are defined in a match block, which is referenced in the match parameter of the health_check directive.
##### Usage
###### In the HTTP Block insert below lines
```
http {
    #...
  match welcome {
    status 200;
    header Content-Type = text/html;
    body ~ "Welcome to nginx!";
}
}
 ```
###### In /conf.d/*.http file mention the 
  health_check match=server_ok; 

```    
   server {
        #...
        location / {
            proxy_pass http://backend;
            health_check match=welcome;
        }
    }
```

### 4# Health check for Status code must not be redirection or Refresh
 ```   
  health_check match=not_redirect; 
 ```
##### Explaination
health_check match=not_redirect : You can set custom conditions that the response must satisfy for the server to pass the health check. The conditions are defined in a match block, which is referenced in the match parameter of the health_check directive.
##### Usage
###### In the HTTP Block insert below lines
```
http {
    #...
      match not_redirect {
    status ! 301-303 307;
    header ! Refresh;
    }
}
 ```
###### In /conf.d/*.http file mention the 
  health_check match=server_ok; 

```    
   server {
        #...
        location / {
            proxy_pass http://backend;
            health_check match=not_redirect;
        }
    }
```



 ### 5# Customized Timeout & Customized Port to do health check on it
 ```   
 health_check;
   health_check port=12346
   health_check_timeout 5s;  
 ```
##### Explaination
health_check : Using this only means it will use the default timeout.
<br /><br />
health_check_timeout 5s : This is the default health check for nginx it will check the upstream/pool every 5 seconds.
<br /><br />
health_check port=12346 : This will check the specific port.
##### Usage
```
http {  
    #...  
    
    server {  
        listen               12345; 
      location / {
        proxy_pass           stream_backend;  
        health_check         port=12346;  
        health_check_timeout 5s;  
        }
    }  
}  
```
Or
```
http {  
    #...  
    server {  
        listen        12345;  
        location / {
        proxy_pass    stream_backend;  
        health_check;  
        #...  
        }
    }  
}  
```

### 6#  Setting the prefered upstream/backend, Setting Max failuers and Setting Max active connections
 ```        
weight=5;
max_fails=2 fail_timeout=30s; 
max_conns=3;  
 ```
 ##### Explaination
weight=5 : the higher the weight value the most preferred <br /><br />
fail_timeout=30s : Sets the time during which a number of failed attempts must happen for the server to be marked unavailable, and also the time for which the server is marked unavailable (default is 10 seconds). <br /><br />
max_fails=2 fail_timeout=30s : Sets the number of failed attempts that must occur during the fail_timeout period for the server to be marked unavailable (default is 1 attempt). <br /><br />
max_conns=3 : limits the maximum number of simultaneous active connections to the proxied server, Default value is zero.
##### Usage
```
upstream stream_backend {  
    server backend1.example.com:12345 weight=5;  
    server backend2.example.com:12345 max_fails=2 fail_timeout=30s;  
    server backend3.example.com:12346 max_conns=3;  
}  
```

### 7# Setting Active and Passive, Setting How to recover from failed server gradually
 ```   
slow_start=30s;  
backup; 
down;
 ```
 ##### Explaination
low_start=30s : An upstream server/backend server can be easily overwhelmed by connections, which may cause the server to be marked as unavailable again. Slow start allows an upstream server to gradually recover its weight from zero to its nominal value after it has been recovered or become available.  <br /><br />
backup : This marks the server as a backup server. It will be passed requests when the primary servers are unavailable.
The parameter cannot be used along with the hash, ip_hash, and random load balancing methods. <br /><br />
down : marks the server as permanently unavailable. 
##### Usage
```
upstream backend {  
    server backend1.example.com:12345 slow_start=30s;  
    server backend2.example.com;  
    server 192.0.0.1 backup;  
    server 192.0.0.2 down;  
} 
```


<br /><br /><br />
## Note:
We use http { 

}
in above examples just to confirm that these health checks used for the http but the http block is already defind in the nginx.conf file which mean no need to define it again in the /conf.d/*.http

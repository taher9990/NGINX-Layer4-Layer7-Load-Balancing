 ### Below are health checks that can be used in side the .stream files
 
 ### 1# Health Check Intervals & Attempts settings
 ```interval=10 will check every 10 seconds```
 ##### Explaination
 passes=2 backend servers must pass 2 checks.<br />
 fails=3 if backend server failed 3 times it will be excluded from the pool
 ##### Usage
```
stream {  
    #...  
    server {  
        listen       12345;  
        proxy_pass   stream_backend;  
        health_check interval=10 passes=2 fails=3;  
    }  
    #...  
}  
```

 ### 2# Customized Timeout & Customized Port to do health check on it
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
stream {  
    #...  
    server {  
        listen               12345;  
        proxy_pass           stream_backend;  
        health_check         port=12346;  
        health_check_timeout 5s;  
    }  
}  
```
Or
```
stream {  
    #...  
    server {  
        listen        12345;  
        proxy_pass    stream_backend;  
        health_check;  
        #...  
    }  
}  
```

### 3#  Setting the prefered upstream/backend, Setting Max failuers and Setting Max active connections
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

### 4# Setting Active and Passive, Setting How to recover from failed server gradually
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
We use 
```
stream { 
  ....
}
```
in above examples just to confirm that these health checks used for the stream but the stream block is already defind in the nginx.conf file which mean no need to define it again in the /conf.d/*.stream

 ### Below are health checks that can be used in side the .stream files
 
 ### 1#
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

 ### 2#
 ```        
   health_check;  
   health_check_timeout 5s;  
 ```
 ##### Explaination
This is the default health check for nginx it will check the upstream/pool every 5 seconds, so to use the default only you can use   health_check;   only if you want to customize it you can use both   health_check;   and   health_check_timeout 5s;   and in below example
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

### 3#
 ```        
weight=5;
max_fails=2 fail_timeout=30s; 
max_conns=3;  
 ```
 ##### Explaination
weight=5 : the higher the weight value the most preferred <br />
fail_timeout=30s : Sets the time during which a number of failed attempts must happen for the server to be marked unavailable, and also the time for which the server is marked unavailable (default is 10 seconds). <br />
max_fails=2 fail_timeout=30s : Sets the number of failed attempts that must occur during the fail_timeout period for the server to be marked unavailable (default is 1 attempt). <br />
max_conns=3; 
##### Usage
```
upstream stream_backend {  
    server backend1.example.com:12345 weight=5;  
    server backend2.example.com:12345 max_fails=2 fail_timeout=30s;  
    server backend3.example.com:12346 max_conns=3;  
}  
```

## Note:
We use stream { 

}
in above examples just to confirm that these health checks used for the stream

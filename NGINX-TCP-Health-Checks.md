 ### Below are health checks that can be used in side the .stream files
 
 
 #### health_check interval=10 passes=2 fails=3;  
 interval=10 will check every 10 seconds
 passes=2 backend servers must pass 2 checks
 fails=3 if backend server failed 3 times it will be excluded from the pool
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

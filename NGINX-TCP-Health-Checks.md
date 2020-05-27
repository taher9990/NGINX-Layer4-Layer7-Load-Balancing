 ### Below are health checks that can be used in side the .stream files
 
 ### 1#
 ```interval=10 will check every 10 seconds```
 ##### Explaination
 passes=2 backend servers must pass 2 checks
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

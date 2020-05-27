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

 ### 2#
 ```        
   health_check;  
   health_check_timeout 5s;  
 ```
 ##### Explaination
This is the default health check for nginx it will check the upstream/pool every 5 seconds 
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




## Note:
We use stream { 

}
in above examples just to confirm that these health checks used for the stream

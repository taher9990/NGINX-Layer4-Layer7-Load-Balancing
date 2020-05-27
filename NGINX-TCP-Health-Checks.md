 ### h
 health_check interval=10 passes=2 fails=3;  
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

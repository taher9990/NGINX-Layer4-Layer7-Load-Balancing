# NGINX-Layer4-Load-Balancing
### Install the latest verion of Nginx on Ubuntu 18.04
<br /> We used nginx version 19
```
sudo wget https://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
sudo vi /etc/apt/sources.list
  deb https://nginx.org/packages/mainline/ubuntu/ bionic nginx
  deb-src https://nginx.org/packages/mainline/ubuntu/ bionic nginx
sudo apt-get remove nginx-common
sudo apt-get update
sudo apt-get install nginx


sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
<br />
<br />
### Below is overview on Folder Structure for NGINX for the important folders that we used
##### but there are other defaul folders we did not mention them below
<br />/etc/nginx/<br />
├── conf.d<br />
│   ├── mail-service-VIP.mail<br />
│   ├── http-service-VIP.http<br />
│   ├── WEB-TCP-L4-VIP.stream<br />
│   └── DB-TCP-L4-VIP.stream<br />
├── nginx.conf<br />
└──<br />
 <br />
 <br />
 <br />
#### So you need to  follow below steps to create new VIP e.g. TCP/UDP VIP <br />
1- Create a new file under /etc/nginx/conf.d/<VIP-Name>.stream<br />
 <br />
  ```touch /etc/nginx/conf.d/DNS-Service-VIP.stream```
 <br />
 <br />
2- Configure the new VIP add below text to /etc/nginx/conf.d/DNS-Service-VIP.stream<br />
 <br />
 ```
  upstream DNS-Service {
       hash $remote_addr consistent;  # This is going to remeber client IP and send him to same DNS server that we served first
       server DNS-SRV1.EXAMPLE.COM:53;
       server DNS-SRV2.EXAMPLE.COM:53;
       server DNS-SRV3.EXAMPLE.COM:53;
       server DNS-SRV4.EXAMPLE.COM:53;
       server DNS-SRV5.EXAMPLE.COM:53;
       server DNS-SRV6.EXAMPLE.COM:53;
     }
   server {
        listen 10.10.100.100 udp;
        proxy_pass DNS-Service;
        proxy_bind 10.10.100.100;
        access_log /var/log/nginx/access.log basic;
        error_log /var/log/nginx/error.log debug;
     }
 ```
  <br />
 3- Now save the file<br />
 4- Test the config <br />
  sudo nginx -t <br />
 5- Restart NGINX<br />
  sudo systemctl restart nginx 



<br />
#### Nginx verion used is 
<br />

```
root@test-f5-tutorial-2:/etc/nginx# nginx -v
nginx version: nginx/1.19.0
root@test-f5-tutorial-2:/etc/nginx# nginx -V
nginx version: nginx/1.19.0
built by gcc 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)
built with OpenSSL 1.1.1  11 Sep 2018
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -fdebug-prefix-map=/data/builder/debuild/nginx-1.19.0/debian/debuild-base/nginx-1.19.0=. -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'

```

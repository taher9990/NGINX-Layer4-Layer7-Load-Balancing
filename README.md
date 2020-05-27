# NGINX-Layer4-Load-Balancing
Below is overview on Folder Structure for NGINX for the important folders that we used,
but there are other defaul folders we did not mention them below
<br />/etc/nginx/<br />
├── conf.d<br />
│   ├── mail-service-VIP.mail<br />
│   ├── http-service-VIP.http<br />
│   └── DB-TCP-L4-VIP.stream<br />
├── sites-available<br />
│   ├── default<br />
├── sites-enabled<br />
│   ├── default -> /etc/nginx/sites-available/default<br />
│   ├── mail-service-VIP.mail -> /etc/nginx/conf.d/mail-service-VIP.mail<br />
│   ├── http-service-VIP.http -> /etc/nginx/conf.d/http-service-VIP.http<br />
│   └── DB-TCP-L4-VIP.stream -> /etc/nginx/conf.d/DB-TCP-L4-VIP.stream<br />




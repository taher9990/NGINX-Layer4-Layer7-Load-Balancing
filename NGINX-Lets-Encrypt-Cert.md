#Install and Configure Let’s Encrypt
####Let’s next set up the free SSL provider Let’s Encrypt to automatically sign certificates for all of the domains we just set up in Nginx. On Ubuntu 18.04, add the PPA and install the certificate scripts with aptitude:
```
add-apt-repository ppa:certbot/certbot

apt-get update

apt-get install certbot python-certbot-nginx
```

####In CentOS 7, we install the EPEL repository and install the certificate helper from there.
```
yum install epel-release

yum install certbot python2-certbot-nginx
```

##### On both systems, we can now read the Nginx configuration and ask the Certbot to assign us some certificates.
```
certbot --nginx
```
##### This will ask you some questions about which domains you would like to use (you can leave the option blank to select all domains) and whether you would like Nginx to redirect traffic to your new SSL (we would!). After it finishes it’s signing process, Nginx should automatically reload its configuration, but in case it doesn’t, reload it manually:
```
systemctl reload nginx
```
##### You can now check the running configuration with:
```
nginx -T
```

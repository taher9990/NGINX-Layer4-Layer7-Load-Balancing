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
##### Or you can generate the certificate with following command
```
sudo certbot --nginx -d example.com -d www.example.com
```
##### This will ask you some questions about which domains you would like to use (you can leave the option blank to select all domains) and whether you would like Nginx to redirect traffic to your new SSL (we would!). After it finishes it’s signing process, Nginx should automatically reload its configuration, but in case it doesn’t, reload it manually:
```
systemctl reload nginx
```
##### You can now check the running configuration with:
```
nginx -T
```
##### Verify letus encrypt cert
```
sudo systemctl status certbot.timer
```
##### To test the renewal Process
```
sudo certbot renew --dry-run
```
##### To test the website certificate
Access below WebSite SSL-LABs - Qualys 
https://www.ssllabs.com/ssltest/
##### Let’s Encrypt certificates are only valid for 90 days from issuance, so we want to ensure that they are automatically renewed. Edit the cron file for the root user by running:
```
crontab -e
```
The cron should look like this:
```
45 2 * * 3,6 certbot renew && systemctl reload nginx
```
###### Once you save this file, every Wednesday and Saturday at 2:45 AM, the certbot command will check for any needed renewals, automatically download and install the certs, followed by a reload of the Nginx configuration.



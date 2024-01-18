# Documentation_Task

Nginx server setup with wordpress site:-
Site URL: https://jai.hopto.org/

1. Hostname changed:-
```sh
hostnamectl set-hostname jai.hopto.org
hostnamectl
```

2. Added Swap 4.0Gi Memory:-
```sh
free -h
sudo swapon --show
fallocate -l 4G /swapfile
ls -lh /swapfile
chmod 600 /swapfile
ls -lh /swapfile
mkswap /swapfile
swapon /swapfile
swapon --show
```
Swap File Permanent
```sh
Back up the /etc/fstab file in case anything goes wrong:
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
3. Installed Nginx Web Server
```sh
apt install nginx
ufw allow 'Nginx HTTP'
systemctl status nginx
systemctl enable nginx
```
4. Installed PHP8.3
```sh
apt-get install ca-certificates apt-transport-https software-properties-common
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install php8.3
php -v
apt install php8.3-mysql php8.3-imap php8.3-ldap php8.3-xml php8.3-curl php8.3-mbstring php8.3-zip
```
5. Installed Mariadb
```sh
apt install mariadb-server
mysql_secure_installation
mysql -u root -p  --->  (Promtp for password and able to login without any issue)
Bye
```
6. DB created for wordpress
```sh
CREATE DATABASE wordpressdb DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'admin@12345';
GRANT ALL ON wordpressdb.* TO 'wordpressuser'@'localhost';
exit
```
7. Downloaded latest wordpress
```sh
cd /var/www/html/
wget https://wordpress.org/latest.zip
unzip latest.zip
```
8. Installed Wordpress + php8.3-fpm
```sh
apt-get install php8.3-fpm
```
Added nginx rules on created wordpress.conf to get work wordpress installation
```sh
location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
        include snippets/fastcgi-php.conf;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
```
Enabled Rewrite rules in site-available
```sh
location / {
    #try_files $uri $uri/ =404;
    try_files $uri $uri/ /index.php$is_args$args;
}
```
9. Wordpress login details
```sh
Website URL: https://jai.hopto.org
Admin URL: https://jai.hopto.org/wp-admin/
User: *******
Password: *********
```
10. Installed SSL on domain: jai.hopto.org
```sh
sudo apt remove certbot  -->  (Removed from server)
sudo snap install --classic certbot
sudo certbot --nginx -d jai.hopto.org

Created symlink directory
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```
11. Installed Redis cache
```sh
sudo apt update
sudo apt install redis-server
vi /etc/redis/redis.conf  -->  (Made changes supervised on to systemd)
systemctl restart redis.service
systemctl enable redis.service
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/67dc6368-41a4-4af4-ab13-b9ad8562d750)

12. Fixed Expired header by using below code in nginx.conf
```sh
location ~* \.(jpg|jpeg|gif|png)$ {
  expires 1y;
}

location ~* \.(css)$ {
  expires 1M;
}

location ~* \.(js|pdf|swf)$ {
  expires 1M;
}
```
13. Reverse Proxy enabled
```sh
gunicorn --workers=2 test:app
```

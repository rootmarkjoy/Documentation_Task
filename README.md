# Documentation_Task

Nginx server setup with wordpress site:-
Site URL: https://jai.hopto.org/

1. Hostname changed:-
```sh
hostnamectl set-hostname jai.hopto.org
hostnamectl
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/c2d9712b-aab8-410d-8dfa-8ed417adf77f)

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
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/168975e3-b283-479d-93cc-e78813c9c75e)

3. Installed Nginx Web Server
```sh
apt install nginx
ufw allow 'Nginx HTTP'
systemctl status nginx
systemctl enable nginx
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/991dc1da-d821-4eb2-ba58-8f659741aed1)

![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/91ff462a-909c-4279-876c-8c830c2ca32b)

4. Installed PHP8.3
```sh
apt-get install ca-certificates apt-transport-https software-properties-common
add-apt-repository ppa:ondrej/php
apt-get update
apt-get install php8.3
php -v
apt install php8.3-mysql php8.3-imap php8.3-ldap php8.3-xml php8.3-curl php8.3-mbstring php8.3-zip
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/1467fdfa-d491-4041-9bf9-46b120ece81d)

5. Installed Mariadb
```sh
apt install mariadb-server
mysql_secure_installation
mysql -u root -p  --->  (Promtp for password and able to login without any issue)
Bye
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/aafcbe48-18c3-4709-9dcd-8185e9342a2d)

6. DB created for wordpress
```sh
CREATE DATABASE wordpressdb DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'admin@12345';
GRANT ALL ON wordpressdb.* TO 'wordpressuser'@'localhost';
exit
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/fb1fab49-3288-4822-88d1-8a7d2c3b183e)

7. Downloaded latest wordpress
```sh
cd /var/www/html/
wget https://wordpress.org/latest.zip
unzip latest.zip
```
To secure wordpress more we have added secret-key under wp-config.php file.  -->  (Secret-key can be generate with the help of this URL: https://api.wordpress.org/secret-key/1.1/salt/)
```sh
define('AUTH_KEY',         'Q7}BQ>[K{mAf|HLeZ.v~Uqcx#Ti]oae28f+Hd-DD1pMn+8-t|JF6nk5<w^6?G-1>');
define('SECURE_AUTH_KEY',  '8z`?-oR._3hai!h8:)!;S|aeVZDMDVC=QTP2ZwVBX&QlFAA>p)1vcE^`aRc4Qfo~');
define('LOGGED_IN_KEY',    'y`^bTDITW7n,iih3sFe.+zm[@&22KGR,^Z|O]KO|~PQGhMgHi]i]+@lk$Df2>.lO');
define('NONCE_KEY',        'NZi*++DA78i+d$$zaKWxBu`YctlsM=dA3Q-?K-?.mz1MzG4i4mh`#f3t0xA#QOSZ');
define('AUTH_SALT',        'W&2?lTVJ|+$mjl.|7Z5Q%/Tz6 HgIpeLPd d%1TPKVs!z,)KVo/NHP!0jp2C&*rG');
define('SECURE_AUTH_SALT', 'Knv.wJ4%4mTF9R~$@:=z~kZ|Mti(K@rQ53`=pK.-X%z5md_?PI4U`wvXJ!fc<H~%');
define('LOGGED_IN_SALT',   '~tY)vp+eeB|=ly (JR{Z-bw^QQ&)SEK92N~R*+0/d^cPc<mA^AwNC]4|NEr{q<6x');
define('NONCE_SALT',       ':5LuP0AgRKCr{+t|MG=5jr&*0^;Ol$)PA?ki{E$,-GqT65HA@|&%(2}f,C[/p4&5');
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/99bc3418-af3b-4520-a7ea-0ada158af307)

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
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/95612645-ba38-46e5-bec6-f54c0bea665e)

![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/ce2e59c4-a010-4d35-9ca7-4e4126276dd8)

9. Wordpress login details
```sh
Website URL: https://jai.hopto.org
Admin URL: https://jai.hopto.org/wp-admin/
User: *******
Password: *********
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/31fc8bd1-5151-455c-92e6-3089a38d43d0)

![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/8f88aa44-73dd-4ba7-94ec-a83a18ac9355)

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
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/6b7142a8-542f-473e-9610-4346a7e991c0)

![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/119c2e1e-c5ef-4bf7-87e3-dec3a0bf895c)

11. Installed Redis cache
```sh
sudo apt update
sudo apt install redis-server
vi /etc/redis/redis.conf  -->  (Made changes supervised on to systemd)
systemctl restart redis.service
```
![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/ac49dc81-300f-4b14-b7e4-194c256dfcbd)

![image](https://github.com/rootmarkjoy/Documentation_Task/assets/45856526/97f8279e-a970-4c81-a5a2-5fdbd32d9007)

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

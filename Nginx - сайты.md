https://losst.pro/nastrojka-ssl-v-nginx-s-lets-encrypt

https://ruvds.com/ru/helpcenter/kak-obezopasit-nginx-s-lets-encrypt-ubuntu-20-04/      !!!!!!!!!!!!!!

  
sudo mkdir /var/www/any-server.ru
sudo nano /var/www/any-server.ru/index.html

sudo mkdir /var/www/admin.any-server.ru
sudo nano /var/www/admin.any-server.ru/index.html

sudo mkdir /var/www/angular.any-server.ru
sudo nano /var/www/angular.any-server.ru/index.html

sudo mkdir /var/www/js.any-server.ru
sudo nano /var/www/js.any-server.ru/index.html

sudo mkdir /var/www/nestjs.any-server.ru
sudo nano /var/www/nestjs.any-server.ru/index.html

sudo mkdir /var/www/universal.any-server.ru
sudo nano /var/www/universal.any-server.ru/index.html

sudo mkdir /var/www/rendertron.any-server.ru
sudo nano /var/www/rendertron.any-server.ru/index.html

_______________________________________________________

  
<!DOCTYPE html>

<html>

<head>

<title>Welcome to any-server.ru !</title>

<style>

body {

width: 35em;

margin: 0 auto;

font-family: Tahoma, Verdana, Arial, sans-serif;

}

</style>

</head>

<body>

<h1>Welcome to any-server.ru !</h1>

<p>If you see this page, the any-server.ru website is working!</p>

</body>

</html>

  

_______________________________________________________

sudo nano /etc/nginx/sites-available/any-server
sudo nano /etc/nginx/sites-available/admin_any-server
sudo nano /etc/nginx/sites-available/angular_any-server
sudo nano /etc/nginx/sites-available/js_any-server
sudo nano /etc/nginx/sites-available/nestjs_any-server
sudo nano /etc/nginx/sites-available/universal_any-server
sudo nano /etc/nginx/sites-available/rendertron_any-server

_______________________________________________________

  

server {

listen 80;

listen [::]:80;

root /var/www/any-server.ru;

index index.html index.htm;

server_name any-server.ru www.any-server.ru;

location / {

try_files $uri $uri/ =404;

}

}

  

__________________________________________________________

  

ln -s /etc/nginx/sites-available/any-server /etc/nginx/sites-enabled

ln -s /etc/nginx/sites-available/admin_any-server /etc/nginx/sites-enabled

ln -s /etc/nginx/sites-available/angular_any-server /etc/nginx/sites-enabled

ln -s /etc/nginx/sites-available/js_any-server /etc/nginx/sites-enabled

ln -s /etc/nginx/sites-available/nestjs_any-server /etc/nginx/sites-enabled

ln -s /etc/nginx/sites-available/universal_any-server /etc/nginx/sites-enabled

ln -s /etc/nginx/sites-available/rendertron_any-server /etc/nginx/sites-enabled
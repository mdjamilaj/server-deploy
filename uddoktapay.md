#UddoktaPay

```
scp ./UddoktaPay.zip root@143.110.188.116:/var/www
cd /var/www && unzip UddoktaPay.zip -d /var/www/uddoktapay
cd /etc/nginx/sites-available && sudo nano uddoktapay.conf
```

```
server {

    root /var/www/uddoktapay;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name payment.tryshop.in;

    location / {
     try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }

}
```
 
```
sudo ln -s /etc/nginx/sites-available/uddoktapay.conf /etc/nginx/sites-enabled/
cd /usr/local/src
wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xvf ioncube_loaders_lin_x86-64.tar.gz
cd ioncube/
php -i | grep  extension_dir
cp /usr/local/src/ioncube/ioncube_loader_lin_7.4.so /usr/lib/php/20190902
echo "zend_extension=ioncube_loader_lin_7.4.so" > /etc/php/7.4/mods-available/ioncube.ini
ln -s /etc/php/7.4/mods-available/ioncube.ini /etc/php/7.4/cli/conf.d/01-ioncube.ini
systemctl restart php7.4-fpm
sudo apt-get install php7.4-curl
sudo certbot --nginx -d payment.tryshop.in
sudo chmod -R 777 /var/www/uddoktapay/app/config/database.php && 
sudo chmod -R 777 /var/www/uddoktapay/app/config/config.php && 
sudo chmod -R 777 /var/www/uddoktapay//app/logs && 
sudo chmod -R 777 /var/www/uddoktapay/app/core && 
sudo chmod -R 777 /var/www/uddoktapay/install/install.uddoktapay
```


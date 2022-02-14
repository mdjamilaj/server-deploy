# copy the repo.zip file and past it inner of var file.

#####create folder
```
mkdir /var/www/client
mkdir /var/www/server
```

##### set post-recive permission

```
sudo chmod +x /var/repo/client.git/hooks/post-receive
sudo chmod +x /var/repo/server.git/hooks/post-receive
```

```
sudo apt-get update 
sudo apt-get install nginx
nano /etc/nginx/sites-available/default 
```

```
server {

    root /var/www/server/public;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name admin.canvabd.com www.admin.canvabd.com;

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


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/admin.canvabd.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/admin.canvabd.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}


server {
    server_name canvabd.com www.canvabd.com;
    # Our Node.js application
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }



    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/canvabd.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/canvabd.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}




server {
    if ($host = canvabd.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    server_name canvabd.com www.canvabd.com;
    listen 80;
    return 404; # managed by Certbot


}server {
    if ($host = admin.canvabd.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot



    server_name admin.canvabd.com www.admin.canvabd.com;
    listen 80;
    return 404; # managed by Certbot


}
```

```
sudo nginx -t
sudo systemctl restart nginx
```

```
curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh -o install_nvm.sh 
nano install_nvm.sh 
bash install_nvm.sh 
source ~/.profile 
nvm ls-remote 
nvm install 12.0.0
nvm use 12.0.0
node -v  
npm -v
```


###Laravel Install

```
sudo apt-get install mysql-server
sudo mysql_secure_installation
sudo apt-get install php-fpm php-mysql php-mbstring
sudo nano /etc/php/7.4/fpm/php.ini
cgi.fix_pathinfo=         set it 0
sudo systemctl restart php7.4-fpm
sudo service nginx restart


cd /var/www/server
apt install composer
composer install --no-dev
sudo apt-get update
sudo apt install php-xml
composer install --no-dev
sudo chown -R :www-data /var/www/server
sudo chmod -R 775 /var/www/server/storage
```
###.env
```

APP_NAME=Laravel
APP_ENV=production
APP_KEY=base64:srWsxWctfTHr57s1nVpzv9HyjyngS3Qh0//gmJzoiiQ=
APP_DEBUG=true
APP_URL=https://localhost:3000
CLIENT_BASE_URL= http://localhost:3000/

LOG_CHANNEL=stack

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=newdb
DB_USERNAME=newuser
DB_PASSWORD=password11111111111111111444444444aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa*A


SHURJOPAY_SERVER_URL =  https://shurjopay.com
MERCHANT_USERNAME =tinams
MERCHANT_PASSWORD =LZRAFsFlQb94
MERCHANT_KEY_PREFIX =TIN



BROADCAST_DRIVER=log
CACHE_DRIVER=file
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=smtptestgm@gmail.com
MAIL_PASSWORD="Ad009257"
MAIL_ENCRYPTION=tls

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1

# Set these up at https://github.com/settings/applications/new
GITHUB_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxx
GITHUB_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
GITHUB_REDIRECT_URL=http://localhost:8000/api/auth/login/github/callback

# Set these up at https://console.developers.google.com/
GOOGLE_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxx
GOOGLE_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
GOOGLE_REDIRECT_URL=http://localhost:8000/api/auth/login/google/callback

# Set these up at https://developers.facebook.com/
FACEBOOK_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxx
FACEBOOK_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
FACEBOOK_REDIRECT_URL=http://localhost:8000/api/auth/login/facebook/callback

# Set these up at https://www.linkedin.com/developers/apps/
LINKEDIN_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxx
LINKEDIN_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
LINKEDIN_REDIRECT_URL=http://localhost:8000/api/auth/login/linkedin/callback

# Set these up at https://apps.twitter.com/
TWITTER_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxx
TWITTER_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWITTER_REDIRECT_URL=http://localhost:8000/api/auth/login/twitter/callback



```
```
chmod -R gu+w storage

chmod -R guo+w storage

php artisan cache:clear

mysql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password11111111111111111444444444aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa*A';
GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
CREATE DATABASE newdb;
php artisan migrate --seed
```

###PM2 install

```
npm install pm2 -g 
cd /var/www/client
npm i
npm run build
pm2 start npm --name "client" -- run start
```







 
 
 
 







### Server Deploy Nuxt Adonis Laravel
 ```
    ssh username@server_ip 
    sudo apt-get update
	sudo apt-get install nginx 
	nano /etc/nginx/sites-available/default 
```
Code
```
server {
    server_name nexxers.com www.nexxers.com;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}

server {
    server_name apis.nexxers.com www.apis.nexxers.com;
    location / {
        proxy_pass http://localhost:3333;
        proxy_http_version 1.1;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
server {
    root /var/www/html/phpbackend/public;
    index index.php index.html index.htm index.nginx-debian.html;
    server_name admin.nexxers.com www.admin.nexxers.com;

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
sudo nginx -t
sudo systemctl restart nginx
```
##### nvm Install

```
 curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh -o install_nvm.sh 
 nano install_nvm.sh 
 bash install_nvm.sh 
 ource ~/.profile 
 nvm ls-remote 
 nvm install 14.17.4 
 nvm use 14.17.4 
 node -v  
 npm -v  
```

##### Install Git

```
 mkdir /home/repo && cd  /home/repo 
 git init --bare 
 nano hooks/post-receive
```
code
```
#!/bin/bash

# Location of our bare repository.
GIT_DIR="/home/repo"

# Where we want to copy our code.
TARGET="/var/www/html/frontend"

while read oldrev newrev ref
do
    # Neat trick to get the branch name of the reference just pushed:
    BRANCH=$(git rev-parse --symbolic --abbrev-ref $ref)

    if [[ $BRANCH == "master" ]];
    then
        # Send a nice message to the machine pushing to this remote repository.
        echo "Push received! Deploying branch: ${BRANCH}..."

        # "Deploy" the branch we just pushed to a specific directory.
        git --work-tree=$TARGET --git-dir=$GIT_DIR checkout -f $BRANCH

        . ~/.nvm/nvm.sh
        nvm use 14.17.4
        cd /var/www/html/frontend
        npm run build
        pm2 restart client
    elif [[ $BRANCH == "phpbackend" ]];
    then
        # Send a nice message to the machine pushing to this remote repository.
        echo "Push received! Deploying branch: ${BRANCH}..."

        # "Deploy" the branch we just pushed to a specific directory.
        git --work-tree=/var/www/html/phpbackend --git-dir=/home/repo checkout -f $BRANCH

        cd /var/www/html/phpbackend
        php artisan optimize:clear
    elif [[ $BRANCH == "nodebackend" ]];
    then
	# Send a nice message to the machine pushing to this remote repository.
        echo "Push received! Deploying branch: ${BRANCH}..."

        # "Deploy" the branch we just pushed to a specific directory.
        git --work-tree=/var/www/html/nodebackend --git-dir=/home/repo checkout -f $BRANCH

        . ~/.nvm/nvm.sh
        nvm use 14.17.4
        cd /var/www/html/nodebackend
        node ace build --production
        cd build
        npm ci --production
        cd ..
        cp .env build/.env
        pm2 restart server

        echo "Not master branch. Skipping."
    else
	echo "branch not match. Skipping."
    fi

        echo "Hello Jamil, Everything work fine!"
done
```
```
sudo chmod +x post-receive
mkdir /var/www/html/frontend
mkdir /var/www/html/phpbackend
mkdir /var/www/html/nodebackend
```
##### Repo add local

```
git remote add prod ssh://root@server_ip/home/repo
git push prod master
```
##### PM2 install
```
 npm install pm2 -g 
 cd /var/www/html/frontend
 pm2 start npm -- start 
```
if use spasific port than run the command

 ```
pm2 start test --interpreter none -- --port 5000
 pm2 start test --interpreter none -- --port your_port 
 pm2 start npm --name "test" -- run start 
```
# Laravel App Setup

```
sudo apt-get install mysql-server
sudo mysql_secure_installation
```
##### php install
```
sudo apt-get install php-fpm php-mysql php-mbstring
sudo nano /etc/php/7.4/fpm/php.ini
```
Search ctrl+w `cgi.fix_pathinfo=` Uncomment & set 0
```
sudo systemctl restart php7.0-fpm
sudo service nginx restart
```
##### Composer Install
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```
data push server
```
cd /var/www/html/phpbackend
composer install --no-dev
sudo apt-get install php-xml
apt-get install --yes zip unzip php-pclzip
composer install --no-dev
 sudo chown -R :www-data /var/www/html/phpbackend
 sudo chmod -R 775 /var/www/html/phpbackend/storage
sudo chmod -R 775 /var/www/html/phpbackend/bootstrap/cache
```
#####Mysql Create Use

```
CREATE USER 'user_name'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON * . * TO 'tinams_admin'@'localhost';
FLUSH PRIVILEGES;

CREATE DATABASE tinams_admin;
SHOW DATABASES;

mysql -u root -p'pass'
```
`nano /var/www/html/phpbackend/.env`

```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:LBvQJm1OSTjPXhrY8b/E7qHixtgW7ugFS+FsoddoWiI=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE="db"
DB_USERNAME="root"
DB_PASSWORD="root"

BROADCAST_DRIVER=log
CACHE_DRIVER=file
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=null
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
```

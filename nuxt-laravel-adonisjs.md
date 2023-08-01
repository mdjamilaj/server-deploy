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
    root /var/www/topupbuzz/server/public;
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
 source ~/.profile 
 nvm ls-remote 
 nvm install 12.0.0
 nvm use 12.0.0 
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
        git --work-tree=/var/www/topupbuzz/server --git-dir=/home/repo checkout -f $BRANCH

        cd /var/www/topupbuzz/server
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

Zip File transfer

```
scp root@206.189.132.184:/var/www/html.zip root@65.20.81.93:/var/www

```

```
sudo chmod +x post-receive
mkdir /var/www/html/frontend
mkdir /var/www/topupbuzz/server
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
 cd /var/www/html
 pm2 start ecosystem.config.js
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
sudo systemctl restart php7.4-fpm
sudo service nginx restart
```
##### Composer Install
```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```
data push server
```
cd /var/www/topupbuzz/server

sudo apt-get update
sudo apt install php-xml
sudo apt-get install php-mbstring
composer update
sudo chown -R :www-data /var/www/topupbuzz/server
sudo chmod -R 775 /var/www/topupbuzz/server/storage
sudo chmod -R 775 /var/www/topupbuzz/server/bootstrap/cache
```
#####Mysql Create Use

```
CREATE USER 'tinams_admin'@'localhost' IDENTIFIED BY '#@009257Ad@#';
GRANT ALL PRIVILEGES ON * . * TO 'tinams_admin'@'localhost';
FLUSH PRIVILEGES;

CREATE DATABASE tinams_admin;
SHOW DATABASES;

mysql -u root -p'pass'
```
`nano /var/www/topupbuzz/server/.env`

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
#Adonisjs Setup

`
nano .env
`
and push the code
```
PORT=3333
HOST=localhost
NODE_ENV=development
APP_KEY=s4ohq8_dPAC4spfX-Swfz1_G1BE9kuE0
DB_CONNECTION=mysql
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER='db_name'
MYSQL_PASSWORD='db_pass'
MYSQL_DB_NAME=tinams_admin
SMTP_HOST='smtp.gmail.com'
SMTP_PORT=465
SMTP_USERNAME='mdjamilaj1@gmail.com'
SMTP_PASSWORD='hbsqbfoffjzfuvoh'
CACHE_VIEWS=false


admin_frontend_url=http://localhost:5000/
GOOGLE_CLIENT_ID=954060416186-6gnhga7859hhlf5lbsfi8turooiljelj.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=QUDTUEGKe9GJvLXooP4M3TGG
FACEBOOK_CLIENT_ID=853392751959368
FACEBOOK_CLIENT_SECRET=3393279459cfcbf617cbe7535b9a14ee
```

my sql access
```
ALTER USER 'tinams_admin'@'localhost' IDENTIFIED WITH mysql_native_password BY '#@009257Ad@#';
flush privileges;
```


#permission

```
find /var/www/topupbuzz/server -type d -exec chmod 755 {} \;
find /var/www/topupbuzz/server -type f -exec chmod 644 {} \;
sudo chmod -R 755 /var/www/topupbuzz/server
```

#Certbot add linux
https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal
```
apt install snapd
snap install core
snap refresh core
sudo snap install --classic certbot  //Don't no it's mandatory.
sudo apt install certbot python3-certbot-nginx
sudo certbot certonly --nginx
sudo certbot --nginx -d example.com -d www.example.com
```


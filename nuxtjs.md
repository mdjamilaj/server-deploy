
# Update apt
```
 sudo apt-get update 

```

# Install Nginx
``` 
sudo apt-get install nginx 

``` 

# Site  availables edit
``` 
nano /etc/nginx/sites-available/default 

``` 

# Set the code

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name _;

        location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }


}
```

``` 
sudo nginx -t

 ```


``` 
sudo systemctl restart nginx

 ```

# nvm Install

```
 curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh -o install_nvm.sh 
 
 ```


```
 nano install_nvm.sh 
 
 ```

```
 bash install_nvm.sh 
 
 ```

```
 ource ~/.profile 
 
 ```

```
 nvm ls-remote 
 
 ```

```
 nvm install 14.17.4 
 
 ```

```
 nvm use 14.17.4 
 
 ```

```
 node -v  
 
 ```

```
 npm -v  
 ```


# PM2 install

```
 npm install pm2 -g 
 
 ```
```
 pm2 start npm -- start 
 
 ```
```
 pm2 start test --interpreter none -- --port 3000 
 
 ```
```
 pm2 start npm --name "test" -- run start 
 
 ```



# Install Git

```
 mkdir /home/repo && cd  /home/repo 
 
 ```
```
 git init --bare 
 
 ```
```
 nano hooks/post-receive 
 
 ```

```
#!/bin/sh
git --work-tree=/var/www/html/frontend --git-dir=/var/repo checkout -f
```

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
    else
        echo "Not master branch. Skipping."
    fi

   . ~/.nvm/nvm.sh
   nvm use 14.17.4
   cd /var/www/html/frontend
   npm run build
   npm run generate
   echo "Hello Jamil, Everything work fine!"

done

````


# press the Command

``` sudo chmod +x post-receive ```

``` mkdir /var/www/html/frontend ```

# Repo add local

```  

git remote add prod ssh://root@206.189.132.184/home/repo

```
















# Extra Code 



```

server {
  index index.html;
    server_name 159.65.138.13;

        location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }




}
server {
  index index.html;
    server_name techflav.com;
root /var/www/html/frontend/dist;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri /index.html;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }

   listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/techflav.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/techflav.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
  index index.html;
    server_name www.techflav.com;

  root /var/www/html/frontend/dist;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri /index.html;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }


    location ~ /\.(?!well-known).* {
        deny all;
    }


    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/techflav.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/techflav.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}


server {
    if ($host = techflav.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    server_name techflav.com;
    listen 80;
    return 404; # managed by Certbot


}

server {
    if ($host = www.techflav.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    server_name www.techflav.com;
    listen 80;
    return 404; # managed by Certbot


}


```
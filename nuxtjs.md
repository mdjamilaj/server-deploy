
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
git remote add prod ssh://root@server_ip/home/repo
```
&
```  
git push prod master
```


#Almost done now just run server
```
ssh username@server_ip
```

# PM2 install

```
 npm install pm2 -g 
 ```
 
```
cd /var/www/html/frontend
```

```
 pm2 start npm -- start 
 ```
if use spasific port than run the command
```
 pm2 start test --interpreter none -- --port 5000
 pm2 start test --interpreter none -- --port your_port 
 ```
```
 pm2 start npm --name "test" -- run start 
 ```



# Multi Domain default

```
#NuxtJS
server {
    server_name example.com www.example.com;
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

}

#Laravel
server {
    root /var/www/server/public;
    index index.php index.html index.htm index.nginx-debian.html;

    server_name admin.example.com www.admin.example.com;

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

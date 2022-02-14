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

###PM2 install

```
npm install pm2 -g 
cd /var/www/client
npm i
npm run build
pm2 start npm --name "client" -- run start
```







 
 
 
 







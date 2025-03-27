
mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && sudo nano ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6y1zRXXSHlesujDlAZuaYJ7WValxdltFcteeMzQ1o6xX2jds/O2MSJAVHgtKuiBtx230ClssbT8y0DhY7C5AdRmMgjZSE9d3bePaaFBmW9e/Mki9mRGcT/L2rl9QHhhtcUP2cS9zPyyB/EP8OrNPt5QhUU+ljY2G+9yejkZLWw2HZtUkLYM5Ktsteis7E9VglBMNORjKMV7TmKdXxtVP2wIyns4DECofCLEJWevx/L7w0kf0PG8nu8uLEESWao9JaVyunRPkV8Mni27MfuJZBXgX98HJ+AkMEIc+T2av/rHn4LsnGKnrwTtxRV32+0CrISYs3kTNZcQRy4Wqt9BUb FC@LAPTOP-1EC2GAFR


chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys



ssh root@139.84.136.104 "zip -r mysql.zip /var/lib/mysql"
scp root@139.84.136.104:mysql.zip root@152.42.231.141:/var/lib
ssh root@152.42.231.141 "rm -rf /var/lib/mysql && cd /var/lib && apt install unzip && unzip mysql.zip -d /var/lib/mysql && mv /var/lib/mysql/var/lib/mysql/* /var/lib/mysql/ && rm -rf /var/lib/mysql/var && sudo chown -R mysql:mysql /var/lib/mysql && sudo chmod -R 755 /var/lib/mysql && sudo systemctl restart mysql"




ssh root@152.42.231.141 "rm -rf /etc/nginx/sites-available && rm -rf /etc/nginx/sites-enabled && rm -rf /etc/letsencrypt"

scp -r root@139.84.136.104:/etc/nginx/sites-available/ root@152.42.231.141:/etc/nginx/
scp -r root@139.84.136.104:/etc/nginx/sites-enabled/ root@152.42.231.141:/etc/nginx/

ssh root@139.84.136.104 "zip -r letsencrypt.zip /etc/letsencrypt"
scp root@139.84.136.104:letsencrypt.zip root@152.42.231.141:/etc
ssh root@152.42.231.141 "cd /etc && unzip letsencrypt.zip -d /etc && sudo mkdir /etc/letsencrypt && mv /etc/etc/letsencrypt/* /etc/letsencrypt"



ssh root@152.42.231.141 "rm -rf /var/www && rm -rf /var/repo"

ssh root@139.84.136.104 "zip -r www.zip /var/www"
scp root@139.84.136.104:www.zip root@152.42.231.141:/var
ssh root@152.42.231.141 "cd /var && sudo mkdir /var/www && unzip www.zip -d /var/www && mv /var/www/var/www/* /var/www && rm -rf /var/www/var && . $HOME/.nvm/nvm.sh && nvm use 22.0.0 && find /var/www -name "ecosystem.config.js" -exec pm2 start {} \; && sudo nginx -t && sudo systemctl restart nginx"



ssh root@139.84.136.104 "zip -r repo.zip /var/repo"
scp root@139.84.136.104:repo.zip root@152.42.231.141:/var
ssh root@152.42.231.141 "cd /var && sudo mkdir /var/repo && unzip repo.zip -d /var/repo && mv /var/repo/var/repo/* /var/repo && rm -rf /var/repo/var"


mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && sudo nano ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC5fxNfzHW0m/puR/oUxRgmRXEIjG1in1S9Knn2D0yKSy/5HUDpgihJiDt+nDgsVU6zuqmMpgJS7iF9FVxBi3RKDfe1rO9DQ3rbPZZUmCGTGYuO6zCvHlvpa2ng99l51qU6+tCiLT4LTeqH4qZwZpcZfEKuIt7vCO/dmtHLCHvrvOFFVjBARa9ayJ2dUlih0OZTe6E/ZNAyKctlB5l9XBmb3rZ+rGuS5VD+KW6kbmnDki8j3iM/OXzJTIICEm37D/l09SDoOrz+5BqO9zcSqpTEI2nueNBLFyxrtI+DuMHUY+DJa5E50biA10fx/th2B1jMBupvqf/xLSQaUf8uFI7lkZ3hxkB3HJzFeF7eFqda9tq/EUsyuTOxnNEvb+bvw2q3Xyqsz+cNPRdNBU+Gb8ZVCKWX1uZrNmz1YkAv27omwnzxe2IruKZrpBdddV0Au/YjYDbe8zwKIYzgEcI/nMFFcICnYnBLSLMS+k3uOSnHXd66cYY/9n0MYhehDSs3rb8jrxbYPXmd2Ifj0Q2lT+orkE+iu5micyO0hVNhM7JS2x4RwwAS/RiqBJQ1OBRCktWOO2A8/KIS4v7S/4X1ZLgIWb13PWrvSjVChI2qJXQ31foAOSirRUda+d8OWGVkqUHARPfFaWwBNmpLpv8kC6SAl3yaz+kMOQ8ofb4vyy2+gQ== FC@DESKTOP-OLT6ODG



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

-  git pull ignore local change:
``` 
  git fetch --all
  git reset --hard origin/master
```

-  re-build and restart adonisjs server:
```
  rm -rf package-lock.json node_modules && npm i --legacy-peer-deps
  node ace build --production --ignore-ts-errors
  cd build
  npm ci --production --legacy-peer-deps
  cd ..
  cp .env build/.env
  pm2 restart server
```


- nginx 
```
cd /etc/nginx/sites-available
sudo nginx -t
sudo systemctl restart nginx
```

- permissions
```
sudo chmod -R 777 /var/www/site
```

- delete old data by time
```
DELETE FROM orders
WHERE created_at < DATE_SUB(NOW(), INTERVAL 1 MONTH);
```

- laravel all users logout
```
rm -rf storage/framework/sessions/*
```

- install mysql2
```
npm install --save mysql2
```

- txtid use but not order (end error)
```
sudo nano /etc/systemd/resolved.conf
[Resolve]
DNS=8.8.8.8 8.8.4.4
FallbackDNS=1.1.1.1 1.0.0.1
sudo systemctl restart systemd-resolved
```

- wallet user only
```
DELETE FROM users
WHERE wallet <= 0;

```

- transfer file
```
scp root@139.84.163.105:/var/www/topupbuzz/pay.zip root@139.84.164.31:/var/www/topupbuzz
```

- redis crush issue fix
```
sudo apt autoremove && sudo apt clean
sudo rm -rf /var/cache/* && sudo rm -rf /var/log/* && rm -rf ~/.cache/google-chrome/* && sudo rm -rf /var/log/*
sudo find /root/.config/google-chrome/DeferredBrowserMetrics -type f -delete
redis-cli flushall
clear
node ace queue:listen
```

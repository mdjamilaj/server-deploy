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

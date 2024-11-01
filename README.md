-  git pull ignore local change:
``` 
  git fetch --all
  git reset --hard origin/master
```

-  re-build and restart adonisjs server:
```
  rm -rf package-lock.json node_modules && npm i --legacy-peer-deps
  node ace build --production
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

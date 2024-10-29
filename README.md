-  git pull ignore local change:
``` 
  git fetch --all
  git reset --hard origin/master
```

-  re-build and restart adonisjs server:
``` 
  node ace build --production
  cd build
  npm ci --production
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

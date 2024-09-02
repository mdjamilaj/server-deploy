#### Some of our important need code

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

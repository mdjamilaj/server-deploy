### Firewall allow command

 ```ufw allow 'Nginx full'```

### File download cors issue apache

```$ nano /etc/apache2/sites-available/000-default-le-ssl.conf```

add the line 


```
<Location "/storage/rmlogs/">
    Header set Access-Control-Allow-Origin "*"
</Location>
```


and run the command 
```
$ cd /etc/apache2/mods-available
$ sudo a2enmod headers
$ /etc/init.d/apache2 restart
```


##Composer Install issue fix

```

php -d memory_limit=-1 /usr/local/bin/composer install

```



##Sometime laravel cors issue are comming by laravel composer lock json file update. So we should re install composer.



### Php Version issue Composer install
```composer install --ignore-platform-req=php```


### Curl Verify cert.pem file issue fix.
https://stackoverflow.com/questions/50345702/laravel-guzzle-curl-error-77-error-setting-certificate-verify-locations



###SSH issue

```   Windows update check and resatrt fix it    ```


### React npm install issue

```
npm install --legacy-peer-deps

```



- [ ] Tailwindcss Issue:

```
rm -rf node_modules && rm -rf package-lock.json && rm -rf nuxt && npm cache clean --force
 && npm install && npm install -D tailwindcss postcss autoprefixer
```

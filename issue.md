#File download cors issue apache

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

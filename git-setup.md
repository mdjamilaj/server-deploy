
#Run those command

``` 
sudo  mkdir pay.git && cd pay.git && git init --bare
nano hooks/post-receive

#!/bin/sh
git --work-tree=/var/www/official-website-of-worldview-pro --git-dir=/var/repo/worldview-pro.git checkout -f


. $HOME/.nvm/nvm.sh
nvm install v14.18.1
nvm use v14.18.1

cd /var/www/official-website-of-worldview-pro

echo ‘post-receive: building…’
npm run build && pm2 restart static

``` 

After setup run this command

``` 
cd hooks
sudo chmod +x post-receive
``` 

